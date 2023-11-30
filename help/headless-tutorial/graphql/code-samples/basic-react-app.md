---
title: Aplicativo básico React
description: Um aplicativo React básico que exibe uma lista de aventuras WKND e seus detalhes
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '92'
ht-degree: 2%

---

# Aplicativo básico React

Este [React](https://reactjs.org/) Este aplicativo demonstra como consultar conteúdo usando APIs AEM GraphQL e consultas persistentes. Este aplicativo renderiza um filtro de Aventuras WKND e, ao selecionar uma aventura, exibe os detalhes completos das aventuras.

Este código:

+ Conecta-se a um serviço de publicação AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Para obter uma análise mais detalhada de como esse aplicativo Next.js é criado, consulte [exemplo de documentação do aplicativo React](../example-apps/react-app.md).
