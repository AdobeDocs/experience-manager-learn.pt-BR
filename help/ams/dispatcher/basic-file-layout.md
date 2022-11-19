---
title: Layout básico do arquivo do AMS Dispatcher
description: Entenda o layout básico do arquivo Apache e Dispatcher.
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1163'
ht-degree: 1%

---


# Layout básico do arquivo

[Índice](./overview.md)

[&lt;- Anterior: O que é &quot;O Dispatcher&quot;](./what-is-the-dispatcher.md)

Este documento explica o conjunto de arquivos de configuração padrão do AMS e o raciocínio por trás desse padrão de configuração

## Estrutura de pastas padrão do Enterprise Linux

No AMS, a instalação base usa o Enterprise Linux como o sistema operacional base. Ao instalar o servidor Web Apache, ele tem um conjunto de arquivos de instalação padrão. Estes são os arquivos padrão que são instalados com os RPMs básicos fornecidos pelo repositório yum

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

Ao seguir e honrar o design / estrutura da instalação, obtemos os seguintes benefícios:

- Mais fácil de suportar um layout previsível
- Automaticamente familiarizado com qualquer pessoa que tenha trabalhado em instalações HTTPD do Enterprise Linux no passado
- Permite ciclos de correção totalmente compatíveis com o sistema operacional sem conflitos ou ajustes manuais
- Evita violações do SELinux de contextos de arquivo rotulados incorretamente

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Observação:</b>
As imagens dos servidores do Adobe Managed Services geralmente têm pequenas unidades raiz do Sistema operacional.  Colocamos nossos dados em um volume separado que normalmente é montado em `/mnt` Então usamos esse volume em vez dos padrões para os seguintes diretórios padrão

`DocumentRoot`
- Padrão:`/var/www/html`
- AMS:`/mnt/var/www/html`

`Log Directory`
- Padrão: `/var/log/httpd`
- AMS: `/mnt/var/log/httpd`

Lembre-se de que os diretórios novos e antigos são mapeados de volta ao ponto de montagem original para eliminar a confusão.
O uso de um volume separado não é vital, mas é digno de nota
</div>

## Complementos do AMS

O AMS é adicionado à instalação base do Apache Web Server.

### Document Roots

Raízes de documento padrão do AMS:
- Autor:
   - `/mnt/var/www/author/`
- Publicação:
   - `/mnt/var/www/html/`
- Manutenção de Catch-All e Health Check
   - `/mnt/var/www/default/`

### Diretórios VirtualHost de Preparo e Ativação

Os diretórios a seguir permitem criar arquivos de configuração com uma área de preparo que você pode trabalhar nos arquivos e ativá-los somente quando estiverem prontos.
- `/etc/httpd/conf.d/available_vhosts/`
   - Esta pasta hospeda todos os arquivos / VirtualHost chamados `.vhost`
- `/etc/httpd/conf.d/enabled_vhosts/`
   - Quando estiver pronto para usar a variável `.vhost` arquivos, você tem dentro do `available_vhosts` simbolizá-los da pasta usando um caminho relativo na `enabled_vhosts` diretory

### Adicional `conf.d` Diretórios

Existem partes adicionais comuns nas configurações do Apache e criamos subdiretórios para permitir uma maneira simples de separar esses arquivos e não ter todos os arquivos em um diretório

#### Diretório de regravações

Esse diretório pode conter todas as `_rewrite.rules` arquivos criados que contêm a sintaxe RewriteRulesyntax típica que envolve os servidores da Web Apache [mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) módulo

- `/etc/httpd/conf.d/rewrites/`

#### Diretório de listas de permissões

Esse diretório pode conter todas as `_whitelist.rules` arquivos criados que contêm o `IP Allow` ou `Require IP`sintaxe que envolve servidores da Web Apache [controles de acesso](https://httpd.apache.org/docs/2.4/howto/access.html)

- `/etc/httpd/conf.d/whitelists/`

#### Diretório de variáveis

Esse diretório pode conter todas as `.vars` arquivos criados que contêm variáveis que podem ser consumidas nos arquivos de configuração

- `/etc/httpd/conf.d/variables/`

### Diretório de configuração específico do módulo Dispatcher

O Apache Web Server é muito extensível e, quando um módulo tem muitos arquivos de configuração, é recomendável criar seu próprio diretório de configuração no diretório base de instalação, em vez de agrupar o padrão.

Seguimos a prática recomendada e criamos nossa própria

#### Diretório do arquivo de configuração do módulo

- `/etc/httpd/conf.dispatcher.d/`

#### Preparando e Ativando Farm

Os diretórios a seguir permitem criar arquivos de configuração com uma área de preparo que você pode trabalhar nos arquivos e ativá-los somente quando estiverem prontos.
- `/etc/httpd/conf.dispatcher.d/available_farms/`
   - Esta pasta hospeda todos os seus `/myfarm {` arquivos chamados `_farm.any`
- `/etc/httpd/conf.dispatcher.d/enabled_farms/`
   - Quando estiver pronto para usar o arquivo farm, você tem dentro da pasta available_farms um link simbólico usando um caminho relativo no diretório enabled_farms

### Adicional `conf.dispatcher.d` Diretórios

Existem partes adicionais que são subseções das configurações do arquivo farm do Dispatcher e criamos subdiretórios para permitir uma maneira simples de separar esses arquivos e não ter todos os arquivos em um diretório

#### Diretório de cache

Este diretório contém todos os `_cache.any`, `_invalidate.any` arquivos criados por você que contêm regras sobre como você deseja que o módulo manipule elementos de cache provenientes de AEM, bem como a sintaxe de regras de invalidação.  Mais detalhes nesta seção estão aqui [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

- `/etc/httpd/conf.dispatcher.d/cache/`

#### Diretório de Cabeçalhos do Cliente

Esse diretório pode conter todas as `_clientheaders.any` arquivos criados por você que contêm listas de cabeçalhos de clientes que você deseja transmitir ao AEM quando uma solicitação entrar.  Mais detalhes nesta seção estão aqui [here](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#specifying-the-http-headers-to-pass-through-clientheaders)

- `/etc/httpd/conf.dispatcher.d/clientheaders/`

#### Diretório de filtros

Esse diretório pode conter todas as `_filters.any` arquivos criados que contêm todas as regras de filtro para bloquear ou permitir que o tráfego pelo Dispatcher alcance AEM

- `/etc/httpd/conf.dispatcher.d/filters/`

#### Diretório de renderizações

Esse diretório pode conter todas as `_renders.any` arquivos criados por você que contêm os detalhes de conectividade para cada servidor de back-end do qual o dispatcher consumirá conteúdo

- `/etc/httpd/conf.dispatcher.d/renders/`

#### Diretório Vhosts

Esse diretório pode conter todas as `_vhosts.any` arquivos criados que contêm uma lista dos nomes de domínio e caminhos para corresponder a um farm específico a um servidor back-end específico

- `/etc/httpd/conf.dispatcher.d/vhosts/`

## Estrutura completa da pasta

O AMS estruturou cada um dos arquivos com extensões personalizadas e com a intenção de evitar problemas / conflitos de namespace e qualquer confusão.

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

## Mantendo o ideal

O Enterprise Linux possui ciclos de correção para o Pacote Apache Webserver (httpd).

Quanto menos arquivos padrão instalados você mudar melhor, por motivos de que se houver correções de segurança ou melhorias de configuração aplicadas por meio do comando RPM / Yum, as correções não serão aplicadas sobre um arquivo alterado.

Em vez disso, cria um `.rpmnew` arquivo ao lado do original.  Isso significa que você perderá algumas alterações desejadas e criará mais lixo nas pastas de configuração.

Ou seja, o RPM durante a instalação da atualização procurará `httpd.conf` se estiver no `unaltered` declarará que *replace* o arquivo e você obterá as atualizações essenciais.  Se a variável `httpd.conf` was `altered` em seguida *não substitui* o arquivo e, em vez disso, ele criará um arquivo de referência chamado `httpd.conf.rpmnew` e as muitas correções desejadas estarão nesse arquivo que não se aplica na inicialização do serviço.

O Enterprise Linux foi configurado corretamente para lidar com esse caso de uso de uma maneira melhor.  Eles fornecem áreas nas quais você pode estender ou substituir os padrões definidos para você.  Dentro da instalação base do httpd, você encontrará o arquivo `/etc/httpd/conf/httpd.conf`e tem sintaxe como:

```
Include conf.modules.d/.conf
IncludeOptional conf.d/.conf
```

A ideia é que o Apache quer que você estenda os módulos e as configurações ao adicionar novos arquivos ao `/etc/httpd/conf.d/` e `/etc/httpd/conf.modules.d/` diretórios com extensão de arquivo de `.conf`

Como o exemplo perfeito ao adicionar o módulo Dispatcher ao Apache, você criaria um módulo `.so` no arquivo ` /etc/httpd/modules/` e inclua-o adicionando um arquivo em `/etc/httpd/conf.modules.d/02-dispatcher.conf` com o conteúdo para carregar o módulo `.so` arquivo

```
LoadModule dispatcher_module modules/mod_dispatcher.so
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Aviso:</b>
não modificamos nenhum arquivo já existente fornecido pelo Apache.  Em vez disso, apenas adicionamos o nosso aos diretórios que eles deveriam ir.
</div><br/>

Agora consumimos nosso módulo em nosso arquivo <b>`/etc/httpd/conf.d/dispatcher_vhost.conf`</b> que inicializa nosso módulo e carrega o arquivo de configuração específico do módulo inicial

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Novamente, você notará que adicionamos arquivos e módulos, mas não alteramos os arquivos originais.  Isso nos fornece a funcionalidade desejada e nos protege contra a falta de correções desejadas, além de manter o mais alto nível de compatibilidade com cada atualização do pacote.

[Next -> Explicação dos arquivos de configuração](./explanation-config-files.md)