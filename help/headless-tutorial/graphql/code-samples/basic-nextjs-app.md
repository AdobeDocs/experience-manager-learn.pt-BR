---
title: Aplicativo básico Next.js
description: Um aplicativo básico Next.js que exibe uma lista de aventuras WKND e seus detalhes
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 0%

---


# Aplicativo básico Next.js

Essa [Next.js](https://nextjs.org/) o aplicativo demonstra como consultar conteúdo usando APIs GraphQL AEM usando consultas persistentes. Este aplicativo renderiza um filtro de Aventuras WKND e, ao selecionar uma aventura, exibe os detalhes completos das aventuras.

Este código:

+ Conecta-se a um serviço de publicação do AEM e não requer autenticação
+ Usa as consultas persistentes de WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Para obter uma análise mais aprofundada de como este aplicativo Next.js é criado, consulte o [exemplo de documentação do aplicativo Next.js](../example-apps/next-js.md).
