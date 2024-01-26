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
duration: 27
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Filtrar o aplicativo SvelteKit

Explore a capacidade das APIs do GraphQL sem periféricos do AEM de listar dados usando uma [SvelteKit](https://kit.svelte.dev/) aplicativo. Este aplicativo SvelteKit cria uma lista de aventuras WKND, que podem ser selecionadas para exibir os detalhes da aventura.

Este código demonstra o uso de Adobe [Cliente AEM Headless para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL no SvelteKit. Este aplicativo usa o `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Os detalhes da aventura são solicitados por meio do `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ Conecta-se a um serviço de publicação AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
