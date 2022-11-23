---
title: Verificação de integridade do Dispatcher do AMS
description: O AMS fornece um script cgi-bin de verificação de integridade que os balanceadores de carga de nuvem serão executados para ver se o AEM está íntegro e deve permanecer em serviço para o tráfego público.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: d6b7d63ba02ca73d6c1674d90db53c6eebab3bd2
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 1%

---

# Verificação de integridade do Dispatcher do AMS

[Índice](./overview.md)

[&lt;- Anterior: Arquivos somente leitura](./immutable-files.md)

Quando você tem um dispatcher de linha de base do AMS instalado, ele vem com algumas licenças.  Um desses recursos é um conjunto de scripts de verificação de integridade.
Esses scripts permitem que o balanceador de carga que encaminha a pilha de AEM saiba quais pernas estão saudáveis e as mantenha em serviço.

![GIF animado mostrando o fluxo de tráfego](assets/load-balancer-healthcheck/health-check.gif "Etapas da verificação de integridade")

## Verificação de Integridade Básica do Balanceador de Carga

Quando o tráfego do cliente chegar pela Internet para acessar sua instância do AEM, ele passará por um balanceador de carga

![A imagem mostra o fluxo de tráfego da Internet para o aem através de um balanceador de carga](assets/load-balancer-healthcheck/load-balancer-traffic-flow.png "balanceador de carga-fluxo-tráfego")

Cada solicitação que passar pelo balanceador de carga arredondará o robin para cada instância.  O balanceador de carga tem um mecanismo de verificação de integridade integrado para garantir que está enviando tráfego para um host saudável.

Normalmente, a verificação padrão é uma verificação de porta para ver se os servidores direcionados no balanceador de carga estão ouvindo o tráfego da porta entram em (ou seja, TCP 80 e 443)

> `Note:` Embora isso funcione, não tem um indicador real se AEM é saudável.  Ele só testa se o Dispatcher (servidor da Web Apache) está ativo e em execução.

## Verificação de integridade do AMS

Para evitar o envio de tráfego para um dispatcher saudável que esteja encaminhando uma instância de AEM não saudável, o AMS criou algumas extras que avaliam a integridade da perna e não apenas do Dispatcher.

![A imagem mostra as diferentes partes para que o exame de integridade funcione](assets/load-balancer-healthcheck/health-check-pieces.png "cheques de saúde")

O exame de saúde compreende as seguintes peças:
- 1 `Load balancer`
- 1 `Apache web server`
- 3 `Apache *VirtualHost* config files`
- 5 `CGI-Bin scripts`
- 1 `AEM instance`
- 1 `AEM package`

Vamos cobrir o que cada peça está configurada para fazer e sua importância

### Pacote de AEM

Para indicar se AEM está funcionando, é necessário fazer uma compilação básica da página e fornecer a página.  O Adobe Managed Services criou um pacote básico que contém a página de teste.  A página testa que o repositório está ativo e que os recursos e o modelo da página podem ser renderizados.

![A imagem mostra o pacote AMS no gerenciador de pacotes CRX](assets/load-balancer-healthcheck/health-check-package.png "pacote de verificação de integridade")

Aqui está a página.  Ele mostrará a ID do repositório da instalação

![A imagem mostra a página Regente do AMS](assets/load-balancer-healthcheck/health-check-page.png "página de verificação de integridade")

> `Note:` Verificamos se a página não pode ser armazenada em cache.  Ele não verificaria o status real se cada vez que retornasse uma página em cache!

Este é o ponto de extremidade de peso leve que podemos testar para ver que o AEM está funcionando.

### Configuração do balanceador de carga

Configuramos os balanceadores de carga para apontar para um ponto de extremidade CGI-BIN em vez de usar uma verificação de porta.

![A imagem mostra a configuração de verificação de integridade do balanceador de carga do AWS](assets/load-balancer-healthcheck/aws-settings.png "aws-lb-settings")

![A imagem mostra a configuração da verificação de integridade do balanceador de carga do Azure](assets/load-balancer-healthcheck/azure-settings.png "azure-lb-settings")

### Hosts Virtuais De Verificação De Integridade Do Apache

#### Host virtual CGI-BIN `(/etc/httpd/conf.d/available_vhosts/ams_health.vhost)`

Este é o `<VirtualHost>` Arquivo de configuração do Apache que permite a execução dos arquivos CGI-Bin.

```
Listen 81
<VirtualHost *:81>
    ServerName	"health"
    ...SNIP...
    ScriptAlias /health/ "/var/www/cgi-bin/health/"
</VirtualHost>
```

> `Note:` arquivos cgi-bin são scripts que podem ser executados.  Pode ser um vetor de ataque vulnerável e esses scripts que o AMS usa não estão acessíveis publicamente, disponíveis apenas para o balanceador de carga para testar.


#### Hosts virtuais de manutenção não íntegros

- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost`
- `/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost`

Esses arquivos são nomeados `000_` como o prefixo de propósito.  Ele é configurado intencionalmente para usar o mesmo nome de domínio do site ativo.  A intenção é que esse arquivo seja ativado quando a verificação de integridade detectar que há um problema com um dos back-end do AEM.  Em seguida, ofereça uma página de erro em vez de apenas um código de resposta HTTP 503 sem página.  Ele vai roubar tráfego do normal `.vhost` porque foi carregado antes disso `.vhost` ao compartilhar o mesmo `ServerName` ou `ServerAlias`.  O resultado é que as páginas destinadas a um determinado domínio vão para o vhost que não está funcionando, em vez do padrão, o tráfego é normal.

Quando os scripts de verificação de integridade são executados, eles fazem logout do status de integridade atual.  Uma vez por minuto, há um cronjob em execução no servidor que procura entradas não íntegras no log.  Se detectar que a instância do autor AEM não está funcionando, ela ativará o link simbólico:

Entrada do registro:

```
# grep "ERROR\|publish" /var/log/lb/health_check.log
E, [2022-11-23T20:13:54.984379 #26794] ERROR -- : AUTHOR -- Exception caught: Connection refused - connect(2)
I, [2022-11-23T20:13:54.984403 #26794]  INFO -- : [checkpublish]-author:0-publish:1-[checkpublish]
```

Cron ao detectar o erro e reagir:

```
# grep symlink /var/log/lb/health_check_reload.log
I, [2022-11-23T20:34:19.213179 #2275]  INFO -- : ADDING VHOST symlink /etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost => /etc/httpd/conf.d/enabled_vhosts/000_unhealthy_author.vhost
```

Você pode controlar se os sites do autor ou publicados podem ter esse erro de carregamento de página configurando a configuração do modo de recarregamento em `/var/www/cgi-bin/health_check.conf`

```
# grep RELOAD_MODE /var/www/cgi-bin/health_check.conf
RELOAD_MODE='author'
```

Opções válidas:
- author
   - Esta é a opção padrão.
   - Isso colocará uma página de manutenção para o autor quando não estiver funcionando
- publicação
   - Essa opção colocará uma página de manutenção para o editor quando ela não estiver funcionando
- all
   - Essa opção colocará uma página de manutenção para o autor ou editor, ou ambos, se não estiverem funcionando
- nenhum
   - Esta opção ignora este recurso da verificação de integridade

Ao analisar a `VirtualHost` para isso, você verá que eles carregam o mesmo documento que uma página de erro para cada solicitação que vem quando ela está ativada:

```
<VirtualHost *:80>
	ServerName	unhealthyauthor
	ServerAlias	${AUTHOR_DEFAULT_HOSTNAME}
	ErrorDocument	503 /error.html
	DocumentRoot	/mnt/var/www/default
	<Directory />
		Options FollowSymLinks
		AllowOverride None
	</Directory>
	<Directory "/mnt/var/www/default">
		AllowOverride None
		Require all granted
	</Directory>
	<IfModule mod_headers.c>
		Header always add X-Dispatcher ${DISP_ID}
		Header always add X-Vhost "unhealthy-author"
	</IfModule>
	<IfModule mod_rewrite.c>
		ReWriteEngine   on
		RewriteCond %{REQUEST_URI} !^/error.html$
		RewriteRule ^/* /error.html [R=503,L,NC]
	</IfModule>
</VirtualHost>
```

O código de resposta ainda é um `HTTP 503`

```
# curl -I https://we-retail.com/
HTTP/1.1 503 Service Unavailable
X-Dispatcher: dispatcher1useast1
X-Vhost: unhealthy-author
```

Em vez de uma página em branco, eles receberão essa página.

![A imagem mostra a página de manutenção padrão](assets/load-balancer-healthcheck/unhealthy-page.png "página não íntegro")

### Scripts CGI-Bin

Há 5 scripts diferentes que podem ser configurados nas configurações do balanceador de carga pelo CSE que alteram o comportamento ou critérios quando um Dispatcher é extraído do balanceador de carga.

#### /bin/checkauthor

Esse script, quando usado, verificará e registrará todas as instâncias nas quais estiver sendo aberto, mas retornará apenas um erro se a variável `author` AEM instância não está funcionando

> `Note:` Lembre-se de que, se a instância de publicação AEM não estivesse funcionando, o dispatcher permaneceria em serviço para permitir que o tráfego flua para a instância de AEM do autor

#### /bin/checkpublish (padrão)

Esse script, quando usado, verificará e registrará todas as instâncias nas quais estiver sendo aberto, mas retornará apenas um erro se a variável `publish` AEM instância não está funcionando

> `Note:` Lembre-se de que, se a instância do autor AEM não estivesse funcionando, o dispatcher permaneceria em serviço para permitir que o tráfego flua para a instância de publicação AEM

#### /bin/checkor

Esse script, quando usado, verificará e registrará todas as instâncias nas quais estiver sendo aberto, mas retornará apenas um erro se a variável `author` ou `publisher` AEM instância não está funcionando

> `Note:` Lembre-se de que se a instância de publicação AEM ou de autor AEM não estivesse funcionando, o dispatcher retiraria o serviço.  Isso significa que se um deles estivesse saudável, ele também não receberia tráfego

#### /bin/checkambos

Esse script, quando usado, verificará e registrará todas as instâncias nas quais estiver sendo aberto, mas retornará apenas um erro se a variável `author` e `publisher` AEM instância não está funcionando

> `Note:` Lembre-se de que, se a instância de publicação AEM ou de autor AEM não estivesse funcionando, o dispatcher não retiraria o serviço.  Isso significa que se um deles não fosse saudável, ele continuaria recebendo tráfego e cometendo erros às pessoas que solicitavam recursos.

#### /bin/health

Este script, quando usado, verificará e registrará todas as instâncias nas quais ele estiver enviando, mas retornará em bom estado, independentemente de AEM estar retornando ou não um erro.

> `Note:` Esse script é usado quando a verificação de integridade não está funcionando como desejado e permite que uma substituição mantenha AEM instâncias no balanceador de carga.