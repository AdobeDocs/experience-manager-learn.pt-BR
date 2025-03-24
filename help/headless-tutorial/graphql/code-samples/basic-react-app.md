---
title: Aplicativo básico React
description: Um aplicativo React básico que exibe uma lista de aventuras WKND e seus detalhes
version: Experience Manager as a Cloud Service
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
duration: 17
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# Aplicativo básico React

Este aplicativo [React](https://reactjs.org/) demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes. Este aplicativo renderiza um filtro de Aventuras WKND e, ao selecionar uma aventura, exibe os detalhes completos das aventuras.

Este código:

+ Conecta-se a um serviço de Publicação do AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Para uma análise mais detalhada de como este aplicativo Next.js é criado, consulte a [documentação de exemplo do aplicativo React](../example-apps/react-app.md).
