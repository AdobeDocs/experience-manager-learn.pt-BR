---
title: Introdução ao AEM Headless - GraphQL
description: Saiba mais sobre as APIs do Experience Manager GraphQL e seus recursos.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 2%

---

# Introdução ao AEM Headless - GraphQL {#getting-started-with-aem-headless}

AEM APIs do GraphQL para fragmentos de conteúdo são compatíveis com cenários CMS sem periféricos, onde aplicativos clientes externos renderizam experiências usando conteúdo gerenciado em AEM.

Uma API moderna de entrega de conteúdo é fundamental para a eficiência e o desempenho de aplicativos de front-end baseados em Javascript. Usar uma REST API apresenta desafios:

* Grande número de solicitações para buscar um objeto de cada vez
* Com frequência, o conteúdo é &quot;excessivamente fornecido&quot;, o que significa que o aplicativo recebe mais do que precisa

Para superar esses desafios, o GraphQL fornece uma API baseada em consultas que permite aos clientes consultar AEM somente o conteúdo necessário e receber usando uma única chamada de API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Este vídeo é uma visão geral da API do GraphQL implementada no AEM. A API do GraphQL no AEM foi projetada principalmente para fornecer AEM fragmentos de conteúdo aos aplicativos downstream como parte de uma implantação sem periféricos.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Introdução ao AEM Headless - GraphQL"
>abstract="Saiba como fornecer Fragmentos de conteúdo usando o GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618?captions=por_br" text="Visão geral do GraphQL no AEM"

## Série de vídeo GraphQL sem periféricos AEM

Saiba mais sobre AEM recursos do GraphQL por meio de uma apresentação detalhada dos Fragmentos de conteúdo e das APIs e ferramentas de desenvolvimento AEM GraphQL.

* [Série de vídeo GraphQL sem periféricos AEM](./video-series/modeling-basics.md)

## Tutorial autônomo do GraphQL AEM

Explore AEM recursos do GraphQL criando um aplicativo React que consome Fragmentos de conteúdo por meio AEM APIs do GraphQL.

* [Tutorial autônomo do GraphQL AEM](./multi-step/overview.md)

## AEM GraphQL versus Serviços de conteúdo AEM

|  | AEM APIs do GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmentos de conteúdo estruturados | Componentes AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes AEM |
| Descoberta de conteúdo | Por consulta GraphQL | Por AEM página |
| Formato de entrega | GraphQL JSON | AEM ComponentExporter JSON |
