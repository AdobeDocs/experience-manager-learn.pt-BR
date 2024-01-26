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
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrar aplicativo Angular

Explore a capacidade das APIs do GraphQL sem periféricos do AEM de filtrar dados usando um [Angular](https://angular.io/) aplicativo. Este aplicativo Angular cria uma lista de Aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso de Adobe [Cliente AEM Headless para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL no Angular. Este aplicativo usa o `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para o `wknd-shared/adventures-by-activity` consulta persistente e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço de publicação AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
