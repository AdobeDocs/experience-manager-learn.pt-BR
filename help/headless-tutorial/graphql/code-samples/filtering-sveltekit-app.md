---
title: Aplicativo Simple SvelteKit
description: Um aplicativo SvelteKit simples que exibe aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Filtrar o aplicativo SvelteKit

Explore a capacidade das APIs AEM Headless do GraphQL de listar dados usando um aplicativo [SvelteKit](https://kit.svelte.dev/). Este aplicativo SvelteKit cria uma lista de aventuras WKND, que podem ser selecionadas para exibir os detalhes da aventura.

Este código demonstra o uso de Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL do SDK. Este aplicativo usa a consulta persistente `wknd-shared/adventures-all` para coletar todas as aventuras e obter uma lista de Tipos de atividade disponíveis. Os detalhes de aventura são solicitados por meio da consulta persistente `wknd-shared/adventures-by-slug`.

Este código:

+ Conecta-se a um serviço AEM Publish e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
