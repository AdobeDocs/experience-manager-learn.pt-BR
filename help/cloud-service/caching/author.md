---
title: Armazenamento em cache do serviço do Autor do AEM
description: Visão geral do armazenamento em cache do serviço do AEM as a Cloud Service Author.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: b8e09820-f1f2-4897-b454-16c0df5a0459
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---

# Autor do AEM

O AEM Author tem armazenamento em cache limitado devido à natureza altamente dinâmica e sensível a permissões do conteúdo que oferece. Em geral, não é recomendável personalizar o armazenamento em cache para o AEM Author e, em vez disso, depender das configurações de cache fornecidas pelo Adobe para garantir uma experiência eficiente.

![Diagrama de visão geral do cache do autor do AEM](./assets/author/author-all.png){align="center"}

Embora a personalização do armazenamento em cache no AEM Author não seja incentivada, é útil compreender que o AEM Author tem uma CDN gerenciada pela Adobe, mas não tem uma AEM Dispatcher. Lembre-se de que todas as configurações do AEM Dispatcher são ignoradas no AEM Author, pois ele não tem um AEM Dispatcher.

## CDN

O serviço do AEM Author usa uma CDN, no entanto, seu objetivo é aprimorar a entrega de recursos do produto e não deve ser configurado extensivamente, deixando-o funcionar como está.

![Diagrama de visão geral do cache de publicação do AEM](./assets/author/author-cdn.png){align="center"}

O CDN do autor do AEM fica entre o usuário final, normalmente um profissional de marketing ou autor de conteúdo, e o autor do AEM. Ele armazena em cache arquivos imutáveis, como ativos estáticos que potencializam a experiência de criação do AEM, e não conteúdo criado.

A CDN do Autor do AEM armazena em cache vários tipos de recursos que podem ser de interesse, incluindo um [TTL personalizável em Consultas Persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) e um [TTL longo em Bibliotecas de Clientes personalizadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### Vida útil do cache padrão

Os seguintes recursos voltados para o cliente são armazenados em cache pela CDN do autor do AEM e têm a seguinte vida útil de cache padrão:

| Tipo de conteúdo | Vida útil do cache padrão da CDN |
|:------------ |:---------- |
| [Consultas persistentes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1 minuto |
| [Bibliotecas de clientes (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 dias |
| [Todo o resto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Não armazenado em cache |


## AEM Dispatcher

O serviço de Autor do AEM não inclui o AEM Dispatcher e usa apenas o [CDN](#cdn) para armazenamento em cache.
