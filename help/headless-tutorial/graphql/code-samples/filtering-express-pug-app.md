---
title: Filtro do aplicativo Express
description: Um aplicativo Express simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
source-git-commit: ac2b3a766caea1013165aedd3478bf859212cc89
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---


# Filtro do aplicativo Express

Explore AEM capacidade das APIs do GraphQL headless para filtrar dados usando uma [Express](https://expressjs.com/) e [Pug](https://pugjs.org/) aplicativo. Este aplicativo Express cria uma lista de aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso do Adobe [Cliente sem cabeçalho AEM para NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) para chamar consultas persistentes do GraphQL usando JavaScript baseado em Node.js. Este aplicativo usa a variável `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a função `wknd-shared/adventures-by-activity` consulta persistente e recupera os detalhes da aventura somente para as aventuras do Tipo de atividade especificado. Os detalhes da Aventura são recuperados do AEM através do `wknd-shared/adventures-by-slug` consulta persistente.

Este código:

+ Conecta-se a um serviço de publicação do AEM e não requer autenticação
+ Usa as consultas persistentes de WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`e `wknd-shared/adventures-by-slug`
