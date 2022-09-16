---
title: Reagir componente
description: Um exemplo de componente React que exibe um Fragmento de conteúdo e ativos de imagem referenciados.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '55'
ht-degree: 3%

---


# Reagir componente

Esse componente React consome um único Fragmento de conteúdo de aventura WKND e exibe seu conteúdo como um banner promocional.

Este código:

+ Conecta a [wknd.site](https://wknd.site)Serviço de publicação do AEM do e não requer autenticação
+ Use a consulta persistente: `wknd-shared/adventures-by-slug`
