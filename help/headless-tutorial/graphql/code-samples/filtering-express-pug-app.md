---
title: Aplicativo Filtering Express
description: Um aplicativo Express simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Aplicativo Filtering Express

Explore a capacidade das APIs do AEM Headless GraphQL de filtrar dados usando um aplicativo [Express](https://expressjs.com/) e [Pug](https://pugjs.org/). Este aplicativo Express cria uma lista de Aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso do [AEM Headless Client para NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) da Adobe para invocar consultas persistentes do GraphQL usando o JavaScript baseado em Node.js. Este aplicativo usa a consulta persistente `wknd-shared/adventures-all` para coletar todas as aventuras e obter uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a consulta persistente `wknd-shared/adventures-by-activity` e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado. Os detalhes de aventura são recuperados do AEM por meio da consulta persistente `wknd-shared/adventures-by-slug`.

Este código:

+ Conecta-se a um serviço de Publicação do AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity` e `wknd-shared/adventures-by-slug`
