---
title: Introdução ao AEM headless - GraphQL
description: Saiba mais sobre as APIs do Experience Manager GraphQL e seus recursos.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# Introdução ao AEM Headless - GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

APIs GraphQL do AEM para fragmentos de conteúdo
O é compatível com cenários de CMS headless, em que aplicativos de clientes externos renderizam experiências usando conteúdo gerenciado no AEM.

Uma API de entrega de conteúdo moderna é essencial para a eficiência e o desempenho de aplicativos de front-end baseados em JavaScript. O uso de uma REST API apresenta desafios:

* Grande número de solicitações para obter um objeto de cada vez
* &quot;entrega excessiva&quot; frequente de conteúdo, o que significa que o aplicativo recebe mais do que precisa

Para superar esses desafios, a GraphQL fornece uma API baseada em consultas, permitindo que os clientes consultem AEM somente quanto ao conteúdo necessário e recebam usando uma única chamada de API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Este vídeo é uma visão geral da API do GraphQL implementada no AEM. A API do GraphQL no AEM foi projetada principalmente para fornecer fragmentos de conteúdo do AEM para aplicativos downstream como parte de uma implantação headless.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introdução ao AEM Headless - GraphQL"
>abstract="Saiba como fornecer Fragmentos de conteúdo usando o GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618?captions=por_br" text="Visão geral do GraphQL no AEM"

## Série de vídeo GraphQL sem periféricos AEM

Saiba mais sobre os recursos de GraphQL do AEM por meio da apresentação detalhada dos Fragmentos de conteúdo e das APIs e ferramentas de desenvolvimento do GraphQL do AEM.

* [Série de vídeo GraphQL sem periféricos AEM](./video-series/modeling-basics.md)

## Tutorial prático do AEM Headless GraphQL

Explore os recursos do GraphQL do AEM criando um aplicativo React que consome fragmentos de conteúdo por meio de APIs do GraphQL do AEM.

* [Tutorial prático do AEM Headless GraphQL](./multi-step/overview.md)

## AEM GraphQL versus AEM Content Services

|                                | APIs do GraphQL para AEM | Serviços de conteúdo AEM |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmentos do conteúdo estruturado | Componentes do AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes do AEM |
| Descoberta de conteúdo | Por consulta do GraphQL | Por página do AEM |
| Formato de entrega | GRAPHQL JSON | JSON do exportador do componente AEM |
