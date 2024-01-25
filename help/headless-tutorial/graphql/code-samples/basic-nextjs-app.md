---
title: Aplicativo básico Next.js
description: Um aplicativo Next.js básico que exibe uma lista de aventuras WKND e seus detalhes
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 25
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# Aplicativo básico Next.js

Este [Next.js](https://nextjs.org/) Este aplicativo demonstra como consultar conteúdo usando APIs AEM GraphQL e consultas persistentes. Este aplicativo renderiza um filtro de Aventuras WKND e, ao selecionar uma aventura, exibe os detalhes completos das aventuras.

Este código:

+ Conecta-se a um serviço de publicação AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

Para obter uma análise mais detalhada de como esse aplicativo Next.js é criado, consulte [exemplo Documentação do aplicativo Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> O Codesandbox.io não suporta a edição do aplicativo Next.js no IDE incorporado. Para editar essa amostra de código, [abra o aplicativo Next.js diretamente no codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
