---
title: Aplicativo SvelteKit Simples
description: Um aplicativo SvelteKit simples que exibe aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# Filtragem do aplicativo SvelteKit

Explore AEM capacidade de APIs do GraphQL headless para listar dados usando uma [SvelteKit](https://kit.svelte.dev/) aplicativo. Este aplicativo SvelteKit cria uma lista de aventuras WKND, que podem ser selecionadas para exibir os detalhes da aventura.

Este código demonstra o uso do Adobe [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL a partir do SvelteKit. Este aplicativo usa a variável `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Os detalhes da Aventura são solicitados por meio do `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ Conecta-se a um serviço de publicação do AEM e não requer autenticação
+ Usa as consultas persistentes de WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
