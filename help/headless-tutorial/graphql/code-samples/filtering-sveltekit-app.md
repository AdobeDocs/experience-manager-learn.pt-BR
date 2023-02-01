---
title: Filtragem do aplicativo SvelteKit
description: Um aplicativo SvelteKit simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Filtragem do aplicativo SvelteKit

Explore AEM capacidade das APIs do GraphQL headless para filtrar dados usando uma [SvelteKit](https://kit.svelte.dev/) aplicativo. Este aplicativo SvelteKit cria uma lista de aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso do Adobe [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL a partir do SvelteKit. Este aplicativo usa a variável `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a função `wknd-shared/adventures-by-activity` consulta persistente e recupera os detalhes da aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço de publicação do AEM e não requer autenticação
+ Usa as consultas persistentes de WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
