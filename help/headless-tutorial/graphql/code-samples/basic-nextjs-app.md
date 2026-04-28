---
title: Aplicativo básico Next.js
description: Um aplicativo Next.js básico que exibe uma lista de aventuras WKND e seus detalhes
version: Experience Manager as a Cloud Service
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
duration: 22
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 5%

---

# Aplicativo básico Next.js

Este aplicativo [Next.js](https://nextjs.org/) demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes. Este aplicativo renderiza um filtro de Aventuras WKND e, ao selecionar uma aventura, exibe os detalhes completos das aventuras.

Este código:

+ Conecta-se a um serviço de Publicação do AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`

>[!IMPORTANT]
>
> O Codesandbox.io não suporta a edição do aplicativo Next.js no IDE incorporado. Para editar esta amostra de código, [abra o aplicativo Next.js diretamente em codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
