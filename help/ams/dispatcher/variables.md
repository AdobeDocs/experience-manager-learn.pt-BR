---
title: Utilização e compreensão de variáveis na configuração do AEM Dispatcher
description: Entenda como usar variáveis nos arquivos de configuração dos módulos do Apache e Dispatcher para elevá-las ao próximo nível.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# Uso e noções básicas sobre variáveis

[Índice](./overview.md)

[&lt;- Anterior: Noções básicas sobre cache](./understanding-cache.md)

Este documento explicará como é possível aproveitar a força das variáveis no Apache Web Server e nos arquivos de configuração do módulo Dispatcher.

## Variáveis

O Apache é compatível com variáveis e, desde a versão 4.1.9 do módulo Dispatcher, ele também é compatível com elas!

Podemos utilizá-los para realizar várias tarefas úteis, como:

- Verifique se tudo que é específico do ambiente não está em linha nas configurações, mas extraído para garantir que os arquivos de configuração do desenvolvimento funcionem em produção com a mesma saída funcional.
- Ative os recursos e altere os níveis de log de arquivos imutáveis que o AMS fornece e não permitirá alterações.
- Alteração que inclui o uso com base em variáveis como `RUNMODE` e `ENV_TYPE`
- Corresponder nomes DNS de `DocumentRoot` e `VirtualHost` entre as configurações do Apache e as configurações do módulo.

## Uso de variáveis de linha de base

Devido ao fato de que os arquivos de linha de base do AMS são somente leitura e imutáveis, há recursos que podem ser desativados e ativados, além de serem configurados ao editar as variáveis que consomem.

### Variáveis de linha de base

As variáveis padrão do AMS são declaradas no arquivo `/etc/httpd/conf.d/variables/ootb.vars`.  Este arquivo não é editável, mas existe para garantir que as variáveis não tenham valores nulos.  Elas são incluídas primeiro e depois de incluir `/etc/httpd/conf.d/variables/ams_default.vars`.  Você pode editar esse arquivo para alterar os valores dessas variáveis ou até mesmo incluir os mesmos nomes e valores de variáveis em seu próprio arquivo!

Aqui está uma amostra do conteúdo do arquivo `/etc/httpd/conf.d/variables/ams_default.vars`:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### Exemplo 1 - Forçar SSL

As variáveis mostradas acima `AUHOR_FORCE_SSL` ou `PUBLISH_FORCE_SSL` podem ser definidas como 1 para ativar as regras de regravação que forçam o redirecionamento dos usuários finais para https, ao entrarem na solicitação http

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

Como você pode ver, as regras de regravação incluem o que tem o código para redirecionar o navegador dos usuários finais, mas a variável definida como 1 é o que permite ou não que o arquivo seja usado

### Exemplo 2 - Nível de registro

As variáveis `DISP_LOG_LEVEL` podem ser usadas para definir o que você deseja ter para o nível de log que é realmente usado na configuração em execução.

Este é o exemplo de sintaxe que existe nos arquivos de configuração da linha de base do ams:

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

Se precisar aumentar o nível de log do Dispatcher, basta atualizar a variável `ams_default.vars` `DISP_LOG_LEVEL` para o nível desejado.

Os valores de exemplo podem ser um número inteiro ou a palavra:

| Nível de registro | Valor inteiro | Valor do Word |
| --- | --- | --- |
| Trace | 4 | trace |
| Depurar | 3 | depurar |
| Informações | 2 | informações |
| Aviso | 1 | avisar |
| Erro | 0 | erro |

### Exemplo 3 - Listas de permissões

As variáveis `AUTHOR_WHITELIST_ENABLED` e `PUBLISH_WHITELIST_ENABLED` podem ser definidas como 1 para engajar regras de regravação que incluam regras para permitir ou proibir o tráfego de usuário final com base no endereço IP.  Alternar esse recurso para precisa ser combinado com a criação de um arquivo de regras de lista de permissões para que ele inclua.

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

Como você pode ver, o `sample_whitelist.rules` força a restrição de IP, mas alternar a variável permite que ela seja incluída no `sample.vhost`

## Onde colocar as variáveis

### Argumentos de Inicialização do Servidor Web

O AMS colocará variáveis específicas do servidor/topologia nos argumentos de inicialização do processo Apache dentro do arquivo `/etc/sysconfig/httpd`

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

Isso não é algo que você pode alterar, mas é útil para aproveitar em seus arquivos de configuração

>[!NOTE]
>
>Devido ao fato de que esse arquivo só é incluído quando o serviço é iniciado.  É necessário reiniciar o serviço para pegar as alterações.  Isso significa que uma recarga não é suficiente, mas uma reinicialização é necessária

### Arquivos de Variáveis (`.vars`)

As variáveis personalizadas fornecidas pelo seu código devem estar em `.vars` arquivos dentro do diretório `/etc/httpd/conf.d/variables/`

Esses arquivos podem ter qualquer variável personalizada que você desejar e alguns exemplos de sintaxe podem ser vistos nos seguintes arquivos de amostra

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

Ao criar seus próprios arquivos de variáveis, nomeie-os de acordo com seu conteúdo e siga os padrões de nomenclatura fornecidos no manual [aqui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  No exemplo acima, você pode ver que o arquivo de variáveis hospeda as diferentes entradas DNS como variáveis a serem usadas nos arquivos de configuração.

## Uso de variáveis

Agora que definiu as variáveis nos arquivos de variáveis, você desejará saber como usá-las corretamente dentro dos outros arquivos de configuração.

Usaremos os arquivos de exemplo `.vars` acima para ilustrar um caso de uso adequado.

Queremos incluir todas as variáveis baseadas em ambiente globalmente, criaremos o arquivo `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

Sabemos que quando o serviço httpd é inicializado, ele obtém as variáveis definidas pelo AMS em `/etc/sysconfig/httpd` e tem o conjunto de variáveis de `ENV_TYPE` e `RUNMODE`

Quando esse arquivo global `.conf` for recebido, ele será recebido antecipadamente, pois a ordem de inclusão dos arquivos em `conf.d` é alfanumérica e a ordem de carregamento significa 000 no nome do arquivo, o que garantirá que ele seja carregado antes dos outros arquivos no diretório.

A instrução include também está usando uma variável no nome do arquivo.  Isso pode alterar qual arquivo ele realmente carregará com base no valor das variáveis `ENV_TYPE` e `RUNMODE`.

Se o valor `ENV_TYPE` for `dev`, o arquivo usado será:

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

Se o valor `ENV_TYPE` for `stage`, o arquivo usado será:

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

Se o valor `RUNMODE` for `preview`, o arquivo usado será:

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

Quando esse arquivo for incluído, ele nos permitirá usar os nomes das variáveis que foram armazenadas no.

Em nosso arquivo `/etc/httpd/conf.d/available_vhosts/weretail.vhost`, podemos trocar a sintaxe normal que funcionava apenas para o desenvolvimento:

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

Com uma sintaxe mais recente que usa a força das variáveis para trabalhar no desenvolvimento, preparo e produção:

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

Em nosso arquivo `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any`, podemos trocar a sintaxe normal que funcionava apenas para o desenvolvimento:

```
"dev.weretail.com" 
"dev.weretail.net"
```

Com uma sintaxe mais recente que usa a força das variáveis para trabalhar no desenvolvimento, preparo e produção:

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

Essas variáveis têm uma enorme quantidade de reutilização para individualizar as configurações em execução sem precisar ter arquivos implantados diferentes por ambiente.  Você essencialmente modela seus arquivos de configuração com o uso de variáveis e inclui arquivos com base em variáveis.

## Exibição de valores de variáveis

Às vezes, ao usar variáveis, precisamos pesquisar para ver quais valores podem estar em nossos arquivos de configuração.  Há uma maneira de exibir as variáveis resolvidas executando os seguintes comandos no servidor:

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

Como as variáveis eram exibidas na configuração compilada do Apache:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

Como as variáveis eram exibidas na configuração compilada do Dispatcher:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

Na saída dos comandos, você verá as diferenças da variável no arquivo de configuração em relação à saída compilada.

Exemplo de configuração

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
    DocumentRoot    ${PUBLISH_DOCROOT} 
```

Agora execute os comandos para ver a saída compilada

Configuração do Apache compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

Configuração do Dispatcher compilada:

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[Próximo -> Liberação](./disp-flushing.md)
