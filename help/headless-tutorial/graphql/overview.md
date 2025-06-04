---
title: Introdução ao AEM Headless — GraphQL
description: Saiba mais sobre as APIs GraphQL do Experience Manager e seus recursos.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '266'
ht-degree: 100%

---

# Introdução ao AEM Headless - GraphQL {#getting-started-with-aem-headless}

As APIs GraphQL do AEM para fragmentos de conteúdo
são compatíveis com cenários de CMS headless, em que os aplicativos clientes externos renderizam experiências usando conteúdo gerenciado no AEM.

Uma API de entrega de conteúdo moderna é essencial para a eficiência e o desempenho de aplicativos de front-end baseados em JavaScript. O uso de uma API REST apresenta desafios:

* Grande número de solicitações para obter um objeto de cada vez
* Faz “entrega excessiva” de conteúdo frequentemente, o que significa que o aplicativo recebe mais do que precisa

Para superar esses desafios, o GraphQL fornece uma API baseada em consultas, permitindo que os clientes consultem o AEM somente quanto ao conteúdo necessário e recebam a resposta usando uma única chamada de API.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Este vídeo é uma visão geral da API GraphQL implementada no AEM. A API GraphQL do AEM foi projetada principalmente para fornecer Fragmentos de conteúdo do AEM para aplicativos downstream como parte de uma implantação headless.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introdução ao AEM Headless - GraphQL"
>abstract="Saiba como fornecer Fragmentos de conteúdo usando o GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618" text="Visão geral do GraphQL no AEM"

## Série de vídeos do GraphQL do AEM Headless

Saiba mais sobre os recursos do GraphQL do AEM por meio da apresentação detalhada dos Fragmentos de conteúdo e das APIs GraphQL e ferramentas de desenvolvimento do AEM.

* [Série de vídeos do GraphQL do AEM Headless](./video-series/modeling-basics.md)

## Tutorial prático do GraphQL do AEM Headless

Explore os recursos do GraphQL do AEM criando um aplicativo React que consome fragmentos de conteúdo por meio das APIs GraphQL do AEM.

* [Tutorial prático do GraphQL do AEM Headless](./multi-step/overview.md)

## GraphQL do AEM x Serviços de conteúdo do AEM

|                                | APIs GraphQL do AEM | Serviços de conteúdo do AEM |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmento de conteúdo estruturado | Componentes do AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes do AEM |
| Descoberta de conteúdo | Por consulta do GraphQL | Por página do AEM |
| Formato de entrega | GraphQL JSON | AEM ComponentExporter JSON |
