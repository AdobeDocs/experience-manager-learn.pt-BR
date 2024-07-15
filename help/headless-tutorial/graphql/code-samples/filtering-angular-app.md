---
title: Filtrar aplicativo Angular
description: Um aplicativo Angular simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrar aplicativo Angular

Explore a capacidade das APIs AEM Headless do GraphQL de filtrar dados usando um aplicativo [Angular](https://angular.io/). Este aplicativo Angular cria uma lista de Aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso de Adobe [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL a partir do Angular. Este aplicativo usa a consulta persistente `wknd-shared/adventures-all` para coletar todas as aventuras e obter uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a consulta persistente `wknd-shared/adventures-by-activity` e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço AEM Publish e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
