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
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 71f1d32c12742cdb644dec50050d147395c3f3b6
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 1%

---

# Mapas do site

Saiba como ajudar a aumentar sua SEO criando mapas de site para o AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Recursos

+ [Documentação do AEM Sitemap](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentação do Apache Sling Sitemap](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentação do Sitemap.org Sitemap](https://www.sitemaps.org/protocol.html)
+ [Documentação do arquivo de índice Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## Configurações

### Configuração OSGi do agendador de mapa do site

Define o [Configuração de fábrica OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para a frequência (usando [expressões cron](http://www.cronmaker.com)) os mapas de site serão regerados e armazenados em cache no AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regra de filtro de permissão do Dispatcher

Permitir solicitações HTTP para arquivos de índice e mapa do site.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regra de reescrita do servidor Web Apache

Garantir `.xml` as solicitações HTTP do mapa do site são roteadas para a página AEM subjacente correta. Se a redução de URL não for usada ou se os Mapeamentos do Sling forem usados para alcançar a redução de URL, essa configuração não será necessária.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
