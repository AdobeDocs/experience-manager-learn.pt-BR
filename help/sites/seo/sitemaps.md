---
title: Mapas do site
description: Saiba como ajudar a impulsionar seu SEO criando mapas de site para o AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 937
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 4%

---

# Mapas do site

Saiba como ajudar a impulsionar seu SEO criando mapas de site para o AEM Sites.

>[!WARNING]
>
>Este vídeo demonstra o uso de URLs relativos no mapa de site. Mapas do site [deve usar URLs absolutos](https://sitemaps.org/protocol.html). Consulte [Configurações](#absolute-sitemap-urls) para saber como ativar URLs absolutos, pois isso não é abordado no vídeo abaixo.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configurações

### URLs absolutos do mapa de site{#absolute-sitemap-urls}

O mapa de site AEM oferece suporte a URLs absolutos usando [Mapeamento do Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Isso é feito criando nós de mapeamento nos serviços do AEM que geram mapas de site (normalmente o serviço de publicação do AEM).

Um exemplo de definição de nó de mapeamento Sling para `https://wknd.com` pode ser definido em `/etc/map/https` do seguinte modo:

| Caminho | Nome da propriedade | Tipo de propriedade | Valor da propriedade |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | String | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | String | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | String | `wknd.com/$1` |

A captura de tela abaixo ilustra uma configuração semelhante, mas para `http://wknd.local` (um mapeamento de nome de host local em execução em `http`).

![Configuração de URLs absolutos do mapa do site](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Configuração OSGi do agendador do mapa do site

Define o [Configuração de fábrica do OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para a frequência (usando [expressões cron](https://cron.help/)) mapas de site são gerados/novamente e armazenados em cache no AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regra de filtro de permissão do Dispatcher

Permitir solicitações HTTP para os arquivos de índice do mapa de site e do mapa de site.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regra de reescrita do Apache WebServer

Assegurar `.xml` as solicitações HTTP do mapa do site são roteadas para a página AEM subjacente correta. Se a redução de URL não for usada ou se os Mapeamentos do Sling forem usados para obter a redução de URL, essa configuração não será necessária.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Recursos

+ [Documentação do mapa do site do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Documentação do Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentação do mapa do site Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentação do arquivo de índice do mapa de site Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cron Helper](https://cron.help/)