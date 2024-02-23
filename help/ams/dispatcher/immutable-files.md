---
title: Arquivos imutáveis ou somente leitura do Dispatcher do AMS
description: Entender por que alguns arquivos são somente leitura ou não editáveis e como fazer as alterações funcionais desejadas
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
duration: 266
source-git-commit: 19beb662b63476f4745291338d944502971638a3
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# Arquivos somente leitura ou imutáveis no AMS

[Índice](./overview.md)

[&lt;- Anterior: Logs comuns](./common-logs.md)

## Descrição

Este documento descreverá quais arquivos estão bloqueados e não devem ser alterados e como fazer as configurações desejadas corretamente.

Quando o AMS provisiona um sistema, ele implementa uma configuração de linha de base que torna tudo funcional e seguro.  São itens que o AMS deseja garantir que permaneçam como uma linha de base de funcionalidade e segurança.  Para fazer isso, alguns arquivos são marcados como somente leitura e imutáveis para evitar que você os altere.

O layout não impede que você altere o comportamento deles e substitua as alterações necessárias.  Em vez de alterar esses arquivos, você sobreporá seu próprio arquivo, que substitui o original.

Isso também permite obter a garantia de que, quando o AMS corrigir os Dispatchers com as correções e os aprimoramentos de segurança mais recentes, eles não alterarão seus arquivos.  Dessa forma, você pode continuar a se beneficiar das melhorias e adotar apenas as alterações desejadas.
![Mostra uma pista de boliche com uma bola rolando pela pista.  A bola tem uma seta com a palavra mostrando-lhe.  As proteções das canaletas estão levantadas e têm as palavras arquivos imutáveis acima delas.](assets/immutable-files/bowling-file-immutability.png "boliches-file-immutability")
Como ilustrado na figura acima, os arquivos imutáveis não impedem que você jogue o jogo.  Eles apenas impedem que você prejudique sua performance e o mantêm na pista.  Este método permite usar alguns recursos muito importantes:

- As personalizações são tratadas em seus próprios espaços seguros
- A sobreposição de alterações personalizadas espelha a dos métodos de sobreposição no AEM
- A correção de configurações do AMS pode ser feita sem alterar as personalizações
- O teste da instalação básica com relação às configurações personalizadas pode ser feito simultaneamente para ajudar a discernir se os problemas são causados por personalizações ou por outros arquivos.


Esta é uma lista típica de arquivos implantados com um Dispatcher:

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

Para determinar quais arquivos são imutáveis, você pode executar o seguinte comando em um Dispatcher para ver:

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

Este é um exemplo de resposta de quais arquivos são imutáveis:

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## Como fazer alterações

### Variáveis

As variáveis permitem fazer alterações funcionais sem alterar os próprios arquivos de configuração.  Determinados elementos da configuração podem ser ajustados com o ajuste dos valores das variáveis.  Um exemplo que podemos destacar do arquivo `/etc/httpd/conf.d/dispatcher_vhost.conf` é mostrado aqui:

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

Veja como a diretiva DispatcherLogLevel tem uma variável de `DISP_LOG_LEVEL` em vez do valor normal que você normalmente veria.  Acima dessa seção do código, você também verá uma instrução Include em um arquivo de variáveis.  O arquivo de variável `/etc/httpd/conf.d/variables/ams_default.vars` é o próximo que queremos ver.  Este é o conteúdo do arquivo de variáveis:

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

Você vê acima que o valor atual de `DISP_LOG_LEVEL` é `info`.  Podemos ajustar para rastrear ou depurar, ou o valor/nível do número de sua escolha.  Agora, em qualquer lugar que controle o nível do log, ele será ajustado automaticamente.

### Método de sobreposição

Você deve entender os arquivos de inclusão de nível superior, pois eles serão o ponto de partida para fazer personalizações.  Para começar com um exemplo simples, temos um cenário em que queremos adicionar um novo nome de domínio que pretendemos apontar para este Dispatcher.  O exemplo de domínio que usaremos is we-retail.adobe.com.  Começaremos copiando um arquivo de configuração existente para um novo, no qual poderemos adicionar as alterações:

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

Copiamos o arquivo aem_publish.vhost existente porque ele já tem tudo o que é necessário para as coisas funcionarem; portanto, não precisamos fazer alterações adicionais.  Agora editamos o novo arquivo weretail.vhost e fazemos as alterações necessárias.

Antes:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Depois:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

Agora que atualizamos nossa `ServerName` e `ServerAlias` para corresponder aos novos nomes de domínio, bem como atualização de outros cabeçalhos de navegação estrutural.  Agora vamos ativar nosso novo arquivo para permitir que o Apache saiba como usar nosso novo arquivo:

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

Agora, o servidor Web Apache sabe que o domínio deve fornecer tráfego, mas ainda precisamos informar ao módulo Dispatcher que ele tem um novo nome de domínio para levar em conta.  Começaremos criando um novo `*_vhost.any` arquivo `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` e dentro desse arquivo colocaremos o nome de domínio que queremos considerar:

```
"we-retail.adobe.com"
```

Agora precisamos criar um novo arquivo de farm que usará nosso novo arquivo de entrada vhost e começaremos copiando um arquivo de início forte para nosso novo arquivo.

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

Vamos mostrar as alterações que precisaremos fazer neste arquivo farm

Antes:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

Depois:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

Agora atualizamos o nome do farm e o &quot;include&quot; utilizado no `/virtualhosts` seção da configuração do farm.  Precisamos habilitar este novo arquivo farm para que ele possa ser usado na configuração em execução:

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

Agora, basta recarregar o serviço do servidor Web e usar o novo domínio.

>[!NOTE]
>
>Observe que alteramos apenas as partes necessárias e aproveitamos o código e os &quot;includes&quot; existentes que vieram com os arquivos de configuração da linha de base.  Precisamos apenas definir o elemento que precisa ser alterado.  Facilita as coisas e permite diminuir o tamanho do código

[Próximo -> Verificação de integridade do Dispatcher](./health-check.md)
