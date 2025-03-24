---
title: Aplicativo Filtrar por preação
description: Um aplicativo Preact simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
exl-id: d2b7e8ab-8bbc-495f-94f1-362ea47b3853
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Aplicativo Filtrar por preação

Explore a capacidade das APIs do AEM Headless GraphQL de filtrar dados usando um aplicativo [Preact](https://preactjs.com/). Este aplicativo Preact cria uma lista de Aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso do [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) da Adobe para invocar consultas persistentes do GraphQL no React. Este aplicativo usa a consulta persistente `wknd-shared/adventures-all` para coletar todas as aventuras e obter uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a consulta persistente `wknd-shared/adventures-by-activity` e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço de Publicação do AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
