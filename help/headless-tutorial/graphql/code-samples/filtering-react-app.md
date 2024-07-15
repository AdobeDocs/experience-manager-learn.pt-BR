---
title: Filtrar o aplicativo React
description: Um aplicativo React simples que filtra aventuras WKND modeladas com Fragmentos de conteúdo.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Filtrar o aplicativo React

Explore a capacidade das APIs AEM Headless do GraphQL de filtrar dados usando um aplicativo [React](https://reactjs.org/). Este aplicativo React cria uma lista de aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso de Adobe [AEM Headless Client para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL no React. Este aplicativo usa a consulta persistente `wknd-shared/adventures-all` para coletar todas as aventuras e obter uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a consulta persistente `wknd-shared/adventures-by-activity` e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço AEM Publish e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
