---
title: Layout básico do arquivo do AMS Dispatcher
description: Entenda o layout básico de arquivo do Apache e Dispatcher.
version: Experience Manager 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 8a3f2bb9-3895-45c6-8bb5-15a6d2aac50e
duration: 308
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 0%

---

# Layout básico do arquivo

[Índice](./overview.md)

[&lt;- Anterior: O que é &quot;A Dispatcher&quot;](./what-is-the-dispatcher.md)

Este documento explica o conjunto de arquivos de configuração padrão do AMS e a filosofia por trás desse padrão de configuração

## Estrutura de pastas padrão do Enterprise Linux

No AMS, a instalação básica usa o Enterprise Linux como o sistema operacional básico. Ao instalar o Apache Webserver, ele tem um arquivo de instalação padrão definido. Estes são os arquivos padrão que são instalados por meio da instalação dos RPMs básicos fornecidos pelo repositório yum

```
/etc/httpd/ 
├── conf 
│   ├── httpd.conf 
│   └── magic 
├── conf.d 
│   ├── autoindex.conf 
│   ├── README 
│   ├── userdir.conf 
│   └── welcome.conf 
├── conf.modules.d 
│   ├── 00-base.conf 
│   ├── 00-dav.conf 
│   ├── 00-lua.conf 
│   ├── 00-mpm.conf 
│   ├── 00-proxy.conf 
│   ├── 00-systemd.conf 
│   └── 01-cgi.conf 
├── logs -> ../../var/log/httpd 
├── modules -> ../../usr/lib64/httpd/modules 
└── run -> /run/httpd
```

Ao seguir e honrar o design/a estrutura de instalação, obtemos os seguintes benefícios:

- Mais fácil oferecer suporte a um layout previsível
- Familiarizado automaticamente com qualquer pessoa que tenha trabalhado em instalações HTTPD do Enterprise Linux no passado
- Permite ciclos de patch totalmente compatíveis com o sistema operacional sem conflitos ou ajustes manuais
- Evita violações do SELinux de contextos de arquivo rotulados incorretamente

>[!BEGINSHADEBOX &quot;Nota&quot;]

As imagens dos servidores Adobe Managed Services normalmente têm pequenas unidades raiz do sistema operacional.  Colocamos nossos dados em um volume separado, que normalmente é montado em `/mnt`
Em seguida, usamos esse volume em vez dos padrões dos seguintes diretórios padrão

`DocumentRoot`
- Padrão:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Padrão: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Lembre-se de que os diretórios antigos e novos são mapeados de volta ao ponto de montagem original para eliminar a confusão.
Usar um volume separado não é vital, mas é digno de nota

>[!ENDSHADEBOX]

## Complementos do AMS

O AMS complementa a instalação básica do Apache Web Server.

### Raízes do documento

Raízes padrão do documento AMS:
- Autor:
   - `/mnt/var/www/author/`
- Publicar:
   - `/mnt/var/www/html/`
- Manutenção de captura e verificação de integridade
   - `/mnt/var/www/default/`

### Preparo e diretórios do VirtualHost ativados

Os diretórios a seguir permitem criar arquivos de configuração com uma área de preparação na qual você pode trabalhar e só ativar quando estiverem prontos.
- `/etc/httpd/conf.d/available_vhosts/`
   - Esta pasta hospeda todos os VirtualHost/arquivos chamados `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Quando estiver pronto para usar os arquivos `.vhost`, você terá dentro da pasta `available_vhosts` o symlink deles usando um caminho relativo para o diretório `enabled_vhosts`

### Diretórios `conf.d` adicionais

Há partes adicionais comuns nas configurações do Apache e criamos subdiretórios para permitir uma maneira limpa de separar esses arquivos e não ter todos os arquivos em um diretório

#### Substitui o Diretório

Este diretório pode conter todos os `_rewrite.rules` arquivos criados por você que contêm a sintaxe típica RewriteRulesyntax que envolve o módulo [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) dos servidores Web Apache

- `/etc/httpd/conf.d/rewrites/`

#### Diretório de listas de permissões

Este diretório pode conter todos os `_whitelist.rules` arquivos que você cria que contêm sua `IP Allow` ou `Require IP`sintaxe típica que envolve os [controles de acesso](https://httpd.apache.org/docs/2.4/howto/access.html) dos servidores Web Apache

- `/etc/httpd/conf.d/whitelists/`

#### Diretório de variáveis

Este diretório pode conter todos os arquivos `.vars` criados por você, que contêm variáveis que podem ser consumidas em seus arquivos de configuração

- `/etc/httpd/conf.d/variables/`

### Diretório de configuração específico do módulo Dispatcher

O Apache Web Server é muito extensível e, quando um módulo tem muitos arquivos de configuração, é recomendável criar seu próprio diretório de configuração no diretório base de instalação, em vez de desorganizar o padrão.

Seguimos as práticas recomendadas e criamos a nossa própria

#### Diretório do arquivo de configuração do módulo

- `/etc/httpd/conf.dispatcher.d/`

#### Preparo e Farm Habilitado

Os diretórios a seguir permitem criar arquivos de configuração com uma área de preparação na qual você pode trabalhar e só ativar quando estiverem prontos.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Esta pasta hospeda todos os arquivos do `/myfarm {` chamados `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Quando estiver pronto para usar o arquivo farm, você terá dentro da pasta available_farms os vinculando através de um caminho relativo para o diretório enabled_farms

### Diretórios `conf.dispatcher.d` adicionais

Há partes adicionais que são subseções das configurações de arquivo do farm do Dispatcher e criamos subdiretórios para permitir uma maneira limpa de separar esses arquivos e não ter todos os arquivos em um diretório

#### Diretório de cache

Esse diretório contém todos os arquivos `_cache.any`, `_invalidate.any` criados por você, que contêm suas regras sobre como você deseja que o módulo manipule elementos de cache provenientes do AEM, bem como a sintaxe de regras de invalidação.  Mais detalhes sobre esta seção estão aqui [aqui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Diretório de Cabeçalhos do Cliente

Esse diretório pode conter todos os arquivos `_clientheaders.any` criados por você que contenham listas de Cabeçalhos do cliente que você deseja passar para a AEM quando entrar uma solicitação.  Mais detalhes sobre esta seção estão [aqui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=pt-BR)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Diretório de filtros

Este diretório pode conter todos os `_filters.any` arquivos criados por você, que contêm todas as regras de filtro para bloquear ou permitir o tráfego pela Dispatcher para alcançar o AEM

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Diretório de renderizações

Este diretório pode conter todos os `_renders.any` arquivos criados por você que contenham os detalhes de conectividade para cada servidor back-end do qual o Dispatcher consumirá conteúdo

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Diretório Vhosts

Este diretório pode conter todos os arquivos `_vhosts.any` criados por você que contenham uma lista dos nomes de domínio e caminhos para corresponder a um farm específico para um servidor back-end específico

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Estrutura completa da pasta

O AMS estruturou cada um dos arquivos com extensões de arquivo personalizadas e com a intenção de evitar problemas/conflitos de namespace e qualquer confusão.

Este é um exemplo de um conjunto de arquivos padrão de uma implantação padrão do AMS:

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
│   ├── 00-base.conf
│   ├── 00-dav.conf
│   ├── 00-lua.conf
│   ├── 00-mpm.conf
│   ├── 00-mpm.conf.old
│   ├── 00-proxy.conf
│   ├── 00-systemd.conf
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
├── logs -> ../../var/log/httpd
├── modules -> ../../usr/lib64/httpd/modules
└── run -> /run/httpd
```

## Mantendo-o ideal

O Enterprise Linux tem ciclos de patch para o Apache Webserver Package (httpd).

Os arquivos padrão menos instalados que você altera são melhores, por razões que, se houver correções de segurança ou melhorias de configuração corrigidas, são aplicadas por meio do comando RPM / Yum, elas não aplicarão as correções sobre um arquivo alterado.

Em vez disso, ele cria um arquivo `.rpmnew` ao lado do original.  Isso significa que você perderá algumas alterações desejadas e criará mais lixo em suas pastas de configuração.

Ou seja, o RPM durante a instalação da atualização observará `httpd.conf`, se estiver no estado `unaltered`, ele *substituirá* o arquivo e você obterá as atualizações vitais.  Se o `httpd.conf` era `altered`, então *não substituirá* o arquivo e, em vez disso, criará um arquivo de referência chamado `httpd.conf.rpmnew`, e as muitas correções desejadas estarão nesse arquivo que não se aplica na inicialização do serviço.

O Enterprise Linux foi configurado adequadamente para lidar com esse caso de uso de maneira melhor.  Elas fornecem áreas em que você pode estender ou substituir os padrões definidos para você.  Dentro da instalação base do httpd você encontrará o arquivo `/etc/httpd/conf/httpd.conf`, que contém a sintaxe como:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

A ideia é que o Apache deseje estender os módulos e as configurações ao adicionar novos arquivos aos diretórios `/etc/httpd/conf.d/` e `/etc/httpd/conf.modules.d/` com uma extensão de arquivo de `.conf`

Como exemplo perfeito ao adicionar o módulo Dispatcher ao Apache, você criaria um arquivo do módulo `.so` em ` /etc/httpd/modules/` e o incluiria adicionando um arquivo em `/etc/httpd/conf.modules.d/02-dispatcher.conf` com o conteúdo para carregar o arquivo do módulo `.so`

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

>[!NOTE]
>
>Não modificamos nenhum arquivo já existente fornecido pelo Apache. Em vez disso, adicionamos o nosso aos diretórios que eles deveriam ir.

Agora consumimos nosso módulo em nosso arquivo <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b>, que inicializa nosso módulo e carrega o arquivo de configuração inicial específico do módulo

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Novamente, você observará que adicionamos arquivos e módulos, mas não alteramos nenhum arquivo original.  Isso nos dá a funcionalidade desejada e nos protege contra a falta de correções de patches desejadas, além de manter o mais alto nível de compatibilidade com cada atualização do pacote.

[Próximo -> Explicação dos arquivos de configuração](./explanation-config-files.md)
