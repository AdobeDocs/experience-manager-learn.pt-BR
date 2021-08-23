---
title: Introdução ao AEM Headless - GraphQL
description: Uma visão geral dos recursos e APIs GraphQL AEM.
feature: Fragmentos de conteúdo, API GraphQL, APIs
topic: Sem periféricos, gerenciamento de conteúdo
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---


# Introdução ao AEM Headless - GraphQL

AEM APIs GraphQL para Fragmentos de conteúdo
O suporta cenários CMS sem periféricos, onde aplicativos cliente externos renderizam experiências usando conteúdo gerenciado em AEM.

Uma API moderna de entrega de conteúdo é fundamental para a eficiência e o desempenho de aplicativos de front-end baseados em Javascript. Usar uma REST API apresenta desafios:

* Grande número de solicitações para buscar um objeto de cada vez
* Com frequência, o conteúdo é &quot;excessivamente fornecido&quot;, o que significa que o aplicativo recebe mais do que precisa

Para superar esses desafios, o GraphQL fornece uma API baseada em consulta que permite que os clientes consultem AEM somente o conteúdo necessário e recebam usando uma única chamada de API.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Este vídeo é uma visão geral da API GraphQL implementada no AEM. A API GraphQL no AEM foi projetada principalmente para fornecer AEM Fragmento de conteúdo aos aplicativos downstream como parte de uma implantação sem periféricos.

## Série de vídeo GraphQL sem periféricos AEM

Saiba mais sobre AEM recursos GraphQL por meio da análise detalhada dos Fragmentos de conteúdo e das APIs GraphQL AEM e ferramentas de desenvolvimento.

* [Série de vídeo GraphQL sem periféricos AEM](./video-series/modeling-basics.md)

## AEM Tutorial autônomo sobre GraphQL

Explore AEM recursos GraphQL criando um aplicativo React que consome Fragmentos de conteúdo por meio AEM APIs GraphQL.

* [AEM Tutorial autônomo sobre GraphQL](./multi-step/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | AEM APIs GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmentos de conteúdo estruturados | Componentes AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes AEM |
| Descoberta de conteúdo | Por consulta GraphQL | Por AEM página |
| Formato de entrega | GraphQL JSON | AEM ComponentExporter JSON |