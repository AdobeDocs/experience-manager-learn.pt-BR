---
title: Redirecionamentos de URL usando configurações sem pipeline
description: Saiba como gerenciar redirecionamentos de URL sem usar o código do AEM ou os pipelines de configuração sempre que precisar adicionar ou atualizar um redirecionamento.
version: Cloud Service
feature: Operations, Dispatcher
topic: Development, Content Management, Administration
role: Architect, Developer, User
level: Beginner, Intermediate
doc-type: Article
duration: 0
last-substantial-update: 2025-02-05T00:00:00Z
jira: KT-15739
thumbnail: KT-15739.jpeg
source-git-commit: f3e1bef93e53de19cf917a915c0fb836f7d3c194
workflow-type: tm+mt
source-wordcount: '954'
ht-degree: 0%

---


# Redirecionamentos de URL usando configurações sem pipeline

Saiba como gerenciar redirecionamentos de URL sem usar o código do AEM ou os pipelines de configuração sempre que precisar adicionar ou atualizar um redirecionamento. Dessa forma, permitir que a equipe de marketing gerencie os redirecionamentos sem precisar de um desenvolvedor.

Há várias opções para gerenciar redirecionamentos de URL no AEM. Para obter mais informações, consulte [redirecionamentos de URL](url-redirection.md). O tutorial foca em criar redirecionamentos de URL como pares de valores chave em um arquivo de texto como [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) e usa a configuração específica do AEM as a Cloud Service para carregá-los no módulo Apache/Dispatcher.

## Pré-requisitos

Para concluir este tutorial, você precisa:

- Ambiente AEM as a Cloud Service com versão **18311 ou superior**.

- O projeto [WKND Sites](https://github.com/adobe/aem-guides-wknd) de amostra deve ser implantado nele.

## Caso de uso do tutorial

Para o propósito da demonstração, vamos supor que a equipe de marketing da WKND esteja lançando uma nova campanha de esqui. Eles gostariam de criar URLs curtos para as páginas de aventura de esqui e gerenciá-los por conta própria como gerenciam o conteúdo.

Com base nos requisitos da equipe de marketing, os redirecionamentos de URL que precisam ser criados são os seguintes.

| URL do Source | URL de direcionamento |
|------------|------------|
| /ski | /us/en/adventures.html |
| /ski/northamerica | /us/en/adventures/downhill-skiing-wyoming.html |
| /ski/costa oeste | /us/en/adventures/tahoe-skiing.html |
| /ski/europa | /us/en/adventures/ski-touring-mont-blanc.html |

Agora, vamos ver como gerenciar esses redirecionamentos de URL e as configurações únicas de Dispatcher necessárias no ambiente do AEM as a Cloud Service.

## Como gerenciar redirecionamentos de URL{#manage-redirects}

Para gerenciar os redirecionamentos de URL, há várias opções disponíveis, vamos explorá-las.

### Arquivo de texto em DAM

Os redirecionamentos de URL podem ser gerenciados como pares de valores chave em um arquivo de texto e carregados no Gerenciamento de ativos digitais (DAM) do AEM.

Por exemplo, os redirecionamentos de URL acima podem ser salvos em um arquivo de texto chamado `skicampaign.txt` e carregados na pasta DAM @ `/content/dam/wknd/redirects`. Após a revisão e a aprovação, a equipe de marketing pode publicar o arquivo de texto.

```
# Ski Campaign Redirects separated by the TAB character
/ski      /us/en/adventures.html
/ski/northamerica  /us/en/adventures/downhill-skiing-wyoming.html
/ski/westcoast   /us/en/adventures/tahoe-skiing.html
/ski/europe          /us/en/adventures/ski-touring-mont-blanc.html
```

### ACS Commons - Redirect Map Manager

O [ACS Commons - Gerenciador de Mapa de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) fornece uma interface amigável para gerenciar os redirecionamentos de URL.

Por exemplo, a equipe de marketing pode criar uma nova página *Mapas de redirecionamento* chamada `SkiCampaign` e adicionar os redirecionamentos de URL acima usando a guia **Editar Entradas**. Os redirecionamentos de URL estão disponíveis em `/etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt`.

![Gerenciador do Mapa de Redirecionamento](./assets/pipeline-free-redirects/redirect-map-manager.png)

>[!IMPORTANT]
>
>O ACS Commons versão **6.7.0 ou superior** é necessário para usar o Gerenciador de Mapa de Redirecionamento. Para obter mais informações, consulte o [ACS Commons - Gerenciador de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html).

### ACS Commons - Gerenciador de redirecionamento

Como alternativa, o [ACS Commons - Gerenciador de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) também fornece uma interface amigável para gerenciar os redirecionamentos de URL.

Por exemplo, a equipe de marketing pode criar uma nova configuração chamada `/conf/wknd` e adicionar os redirecionamentos de URL acima usando o botão **+ Configuração de redirecionamento**. Os redirecionamentos de URL estão disponíveis em `/conf/wknd/settings/redirects.txt`.

![Gerenciador de redirecionamento](./assets/pipeline-free-redirects/redirect-manager.png)

>[!IMPORTANT]
>
>O ACS Commons versão **6.10.0 ou superior** é necessário para usar o Gerenciador de Redirecionamento. Para obter mais informações, consulte o [ACS Commons - Gerenciador de Redirecionamento](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html).

## Como configurar o Dispatcher

Para carregar os redirecionamentos de URL como um RewriteMap e aplicá-los às solicitações recebidas, as seguintes configurações de Dispatcher são necessárias.

### Habilitar o módulo Dispatcher para o modo flexível

Primeiro, verifique se o módulo Dispatcher está habilitado para o _modo flexível_. A presença do arquivo `USE_SOURCES_DIRECTLY` na pasta `dispatcher/src/opt-in` indica que o Dispatcher está no modo flexível.

### Carregar redirecionamentos de URL como RewriteMap

Em seguida, crie um novo arquivo de configuração `managed-rewrite-maps.yaml` na pasta `dispatcher/src/opt-in` com a seguinte estrutura.

```yaml
maps:
- name: <MAPNAME>.map # e.g. skicampaign.map
    path: <ABSOLUTE_PATH_TO_URL_REDIRECTS_FILE> # e.g. /content/dam/wknd/redirects/skicampaign.txt, /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt, /conf/wknd/settings/redirects.txt
    wait: false # Optional, default is false, when true, the Apache waits for the map to be loaded before starting
    ttl: 300 # Optional, default is 300 seconds, the reload interval for the map
```

Durante a implantação, o Dispatcher cria o arquivo `<MAPNAME>.map` na pasta `/tmp/rewrites`.

>[!IMPORTANT]
>
> O nome do arquivo (`managed-rewrite-maps.yaml`) e o local (`dispatcher/src/opt-in`) devem ser exatamente como mencionado acima. Pense nele como uma convenção a ser seguida.

### Aplicar redirecionamentos de URL a solicitações recebidas

Finalmente, crie ou atualize o arquivo de configuração de regravação do Apache para usar o mapa acima (`<MAPNAME>.map`). Por exemplo, vamos usar o arquivo `rewrite.rules` da pasta `dispatcher/src/conf.d/rewrites` para aplicar os redirecionamentos de URL.

```
...
# Use the RewriteMap to define the URL redirects
RewriteMap <MAPALIAS> dbm=sdbm:/tmp/rewrites/<MAPNAME>.map

RewriteCond ${<MAPALIAS>:$1} !=""
RewriteRule ^(.*)$ ${<MAPALIAS>:$1|/} [L,R=301]    
...
```

### Exemplo de configurações

Vamos analisar as configurações do Dispatcher para cada uma das opções de gerenciamento de redirecionamento de URL mencionadas [acima](#manage-redirects).

>[!BEGINTABS]

>[!TAB Arquivo de texto em DAM]

Quando os redirecionamentos de URL são gerenciados como pares de valores chave em um arquivo de texto e carregados no DAM, as configurações são as seguintes.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```yaml
maps:
- name: skicampaign.map
  path: /content/dam/wknd/redirects/skicampaign.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```
...

# The DAM-managed skicampaign.txt file as skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!TAB ACS Commons - Gerenciador de Mapa de Redirecionamento]

Quando os redirecionamentos de URL são gerenciados usando o ACS Commons - Redirect Map Manager, as configurações são as seguintes.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```yaml
maps:
- name: skicampaign.map
  path: /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```
...

# The Redirect Map Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!TAB ACS Commons - Gerenciador de redirecionamento]

Quando os redirecionamentos de URL são gerenciados usando o ACS Commons - Redirect Manager, as configurações são as seguintes.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```yaml
maps:
- name: skicampaign.map
  path: /conf/wknd/settings/redirects.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```
...

# The Redirect Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!ENDTABS]

## Como implantar as configurações

>[!IMPORTANT]
>
>O termo *sem pipeline* é usado para enfatizar que as configurações são *implantadas apenas uma vez* e a equipe de marketing pode gerenciar os redirecionamentos de URL atualizando o arquivo de texto.

Para implantar as configurações, use o pipeline de [pilha completa](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline) ou [configuração da camada da Web](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#web-tier-config-pipelines) na [Cloud Manager](https://my.cloudmanager.adobe.com/).

![Implantar via pipeline de pilha completa](./assets/pipeline-free-redirects/deploy-full-stack-pipeline.png)


Depois que a implantação for bem-sucedida, os redirecionamentos de URL estarão ativos e a equipe de marketing poderá gerenciá-los sem precisar de um desenvolvedor.

## Como testar os redirecionamentos de URL

Vamos testar os redirecionamentos de URL usando o navegador ou o comando `curl`. Acesse a URL `/ski/westcoast` e verifique se ela é redirecionada para `/us/en/adventures/tahoe-skiing.html`.


## Resumo

Neste tutorial, você aprendeu a gerenciar redirecionamentos de URL usando configurações sem pipeline no ambiente do AEM as a Cloud Service.

A equipe de marketing pode gerenciar os redirecionamentos de URL como pares de valores chave em um arquivo de texto e carregá-los no DAM ou usar o ACS Commons - Gerenciador de mapa de redirecionamento ou Gerenciador de redirecionamento. As configurações do Dispatcher são atualizadas para carregar os redirecionamentos de URL como um RewriteMap e aplicá-los às solicitações recebidas.

