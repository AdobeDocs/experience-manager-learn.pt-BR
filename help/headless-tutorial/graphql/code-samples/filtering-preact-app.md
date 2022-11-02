---
title: Filtro do aplicativo Preact
description: Um aplicativo Preact simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: a21b78456354c18ad137e69a5d18258d652169b1
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Filtro do aplicativo Preact

Explore AEM capacidade das APIs GraphQL sem cabeçalho de filtrar dados usando um [Preact](https://preactjs.com/) aplicativo. Este aplicativo Preact cria uma lista de aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso do Adobe [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas GraphQL persistentes do React. Este aplicativo usa a variável `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a função `wknd-shared/adventures-by-activity` consulta persistente e recupera os detalhes da aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço de publicação do AEM e não requer autenticação
+ Usa as consultas persistentes de WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
