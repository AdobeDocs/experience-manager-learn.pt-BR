---
title: Mapas do site
description: Saiba como ajudar a aumentar sua SEO criando mapas de site para o AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# Mapas do site

Saiba como ajudar a aumentar sua SEO criando mapas de site para o AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Recursos

+ [Documentação do AEM Sitemap](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentação do Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [GitHub dos componentes principais do AEM WCM](https://github.com/adobe/aem-core-wcm-components)
   + Funcionalidade de mapa de site adicionada na v2.17.6
+ [Documentação do Sitemap.org Sitemap](https://www.sitemaps.org/protocol.html)
+ [Documentação do arquivo de índice Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## Configurações

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

Define o [Configuração de fábrica OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para a frequência (usando [expressões cron](http://www.cronmaker.com)) os mapas de site serão regerados e armazenados em cache no AEM.

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

Permitir solicitações HTTP para arquivos de índice e mapa do site.

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

Garantir `.xml` as solicitações HTTP do mapa do site são roteadas para a página AEM subjacente correta. Se a redução de URL não for usada ou se os Mapeamentos do Sling forem usados para alcançar a redução de URL, essa configuração não será necessária.

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
