---
title: Como usar e entender variáveis na sua configuração do Dispatcher do AEM
description: Entenda como usar variáveis nos arquivos de configuração dos módulos Apache e Dispatcher para levá-las ao próximo nível.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---


# Uso e noções básicas sobre variáveis

[Índice](./overview.md)

[&lt;- Anterior: Noções básicas sobre cache](./understanding-cache.md)

Este documento explicará como você pode aproveitar o potencial das variáveis no servidor da Web Apache e nos arquivos de configuração do módulo Dispatcher.

## Variáveis

O Apache é compatível com variáveis e, desde a versão 4.1.9 do módulo Dispatcher, também é compatível com elas!

Podemos aproveitá-los para fazer uma série de coisas úteis como:

- Certifique-se de que qualquer item específico do ambiente não esteja em linha nas configurações, mas seja extraído para garantir que os arquivos de configuração do dev funcionem na produção com a mesma saída funcional.
- Alterne os recursos e altere os níveis de log de arquivos imutáveis que o AMS fornece e não permite que você altere.
- Alterar as inclusões que devem ser usadas com base em variáveis como `RUNMODE` e `ENV_TYPE`
- Corresponder `DocumentRoot`&#39;s e `VirtualHost` Nomes DNS entre configurações do Apache e configurações do módulo.

## Uso das variáveis de linha de base

Devido ao fato de os arquivos de linha de base do AMS serem somente leitura e imutáveis, há recursos que podem ser desativados e ativados, bem como configurados ao editar as variáveis que consomem.

### Variáveis da linha de base

As variáveis padrão do AMS são declaradas no arquivo `/etc/httpd/conf.d/variables/ootb.vars`.  Este arquivo não é editável, mas existe para garantir que as variáveis não tenham valores nulos.  Eles são incluídos primeiro depois do que incluímos `/etc/httpd/conf.d/variables/ams_default.vars`.  Você pode editar esse arquivo para alterar os valores dessas variáveis ou até mesmo incluir os mesmos nomes e valores de variáveis em seu próprio arquivo!

Esta é uma amostra do conteúdo do arquivo `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Exemplo 1 - Forçar SSL

As variáveis mostradas acima `AUHOR_FORCE_SSL`ou `PUBLISH_FORCE_SSL` pode ser definido como 1 para ativar as regras de regravação que forçam os usuários finais ao entrarem na solicitação http a serem redirecionados para https

Esta é a sintaxe do arquivo de configuração que permite que essa alternância funcione:

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

Como você pode ver, a inclusão das regras de regravação tem o código para redirecionar o navegador dos usuários finais, mas a variável que está sendo definida como 1 é a que permite que o arquivo seja usado ou não

### Exemplo 2 - Nível de registro

As variáveis `DISP_LOG_LEVEL` pode ser usada para definir o que você deseja ter para o nível de log que é realmente usado na configuração em execução.

Este é o exemplo de sintaxe que existe nos arquivos de configuração da linha de base do AMS:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Se você precisar aumentar o nível de log do Dispatcher, basta atualizar a `ams_default.vars` variável `DISP_LOG_LEVEL` ao nível que você gostaria.

Exemplo de valores pode ser um inteiro ou a palavra:

| Nível de registro | Valor inteiro | Valor da palavra |
| --- | --- | --- |
| Rastreio | 4 | traçar |
| Depurar | 3 | depurar |
| Informações | 2 | informações |
| Aviso | 1 | aviso |
| Erro | 0 | erro |

### Exemplo 3 - Listas de permissões

As variáveis `AUTHOR_WHITELIST_ENABLED` e `PUBLISH_WHITELIST_ENABLED` pode ser definido como 1 para engajar regras de regravação que incluem regras para permitir ou impedir o tráfego do usuário final com base no endereço IP.  A alternância desse recurso precisa ser combinada com a criação de um arquivo de regras da lista de permissões, bem como para incluir.

Estes são alguns exemplos de sintaxe de como a variável habilita as inclusões dos arquivos da lista de permissões e um exemplo de arquivo da lista de permissões

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

Como você pode ver o `sample_whitelist.rules` aplica a restrição de IP, mas a alternância da variável permite que ela seja incluída no `sample.vhost`

## Onde colocar as variáveis

### Argumentos de inicialização do servidor da Web

O AMS colocará variáveis específicas de servidor / topologia nos argumentos de inicialização do processo Apache dentro do arquivo `/etc/sysconfig/httpd`

Esse arquivo tem variáveis predefinidas, como mostrado aqui:

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

Isso não é algo que você pode alterar, mas que pode ser aproveitado nos arquivos de configuração

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Devido ao fato de que esse arquivo só é incluído quando o serviço é iniciado.  É necessário reiniciar o serviço para coletar as alterações.  Isso significa que uma recarga não é suficiente, mas uma reinicialização é necessária
</div>

### Arquivos de variáveis (`.vars`)

As variáveis personalizadas fornecidas pelo código devem estar ativas em `.vars` arquivos dentro do diretório `/etc/httpd/conf.d/variables/`

Esses arquivos podem ter as variáveis personalizadas desejadas e alguns exemplos de sintaxe podem ser vistos nos seguintes arquivos de amostra

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

Ao criar seus próprios arquivos de variáveis, nomeie-os de acordo com seu conteúdo e siga os padrões de nomenclatura fornecidos no manual [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  No exemplo acima, você pode ver que o arquivo de variáveis hospeda as diferentes entradas DNS como variáveis a serem usadas nos arquivos de configuração.

## Uso de variáveis

Agora que você definiu suas variáveis dentro dos arquivos de variáveis, será necessário saber como usá-las corretamente em outros arquivos de configuração.

Usaremos o exemplo `.vars` arquivos acima para ilustrar um caso de uso adequado.

Queremos incluir todas as variáveis baseadas em ambiente globalmente, criaremos o arquivo `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Sabemos que, quando o serviço httpd é iniciado, ele recebe as variáveis definidas pelo AMS em `/etc/sysconfig/httpd` e tem o conjunto de variáveis de `ENV_TYPE` e `RUNMODE`

Quando este `.conf` o arquivo é recebido e recebido antecipadamente porque a ordem de inclusão dos arquivos no `conf.d` é alfanumérico ordem de carregamento médio 000 no nome do arquivo garante que seja carregado antes dos outros arquivos no diretório.

A instrução include também está usando uma variável no nome do arquivo.  Isso pode alterar o arquivo que ele realmente carregará com base no valor na variável `ENV_TYPE` e `RUNMODE` variáveis.

Se a variável `ENV_TYPE` valor é `dev` então o arquivo usado é:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Se a variável `ENV_TYPE` valor é `stage` então o arquivo usado é:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Se a variável `RUNMODE` valor é `preview` então o arquivo usado é:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Quando esse arquivo for incluído, ele nos permitirá usar os nomes de variáveis que foram armazenados no .

Em nosso `/etc/httpd/conf.d/available_vhosts/weretail.vhost` podemos trocar a sintaxe normal que funcionava apenas para o desenvolvimento:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Com uma sintaxe mais recente que usa o poder das variáveis para funcionar no desenvolvimento, estágio e produção:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

Em nosso `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` podemos trocar a sintaxe normal que funcionava apenas para o desenvolvimento:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Com uma sintaxe mais recente que usa o poder das variáveis para funcionar no desenvolvimento, estágio e produção:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Essas variáveis têm uma grande quantidade de reutilização para individualizar as configurações de execução sem precisar ter arquivos implantados diferentes por ambiente.  Basicamente, modele seus arquivos de configuração com o uso de variáveis e inclua arquivos com base em variáveis.

## Exibição dos valores da variável

Às vezes, ao usar variáveis, precisamos pesquisar para ver quais valores podem estar em nossos arquivos de configuração.  Há uma maneira de exibir as variáveis resolvidas executando os seguintes comandos no servidor:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Como as variáveis pareciam em sua configuração compilada do Apache:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Como as variáveis pareciam na configuração compilada do Dispatcher:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Na saída dos comandos, você verá as diferenças da variável no arquivo de configuração em relação à saída compilada.

Exemplo de configuração

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

Agora execute os comandos para ver a saída compilada

Configuração do Apache Compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configuração do Dispatcher Compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Próximo -> Fluxo](./disp-flushing.md)