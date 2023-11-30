---
title: Filtrando com jQuery e Handlebars
description: Uma implementação JavaScript usando jQuery e Handlebars que filtra as Aventuras WKND a serem exibidas. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Filtrando com jQuery e Handlebars

Explore a capacidade das APIs do GraphQL do AEM Headless de filtrar dados usando um aplicativo JavaScript que usa [jQuery](https://jquery.com/) e [Handlebars](https://handlebarsjs.com/). Este aplicativo cria uma lista de Aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso de Adobe [Cliente AEM Headless para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL. Este aplicativo usa o `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para o `wknd-shared/adventures-by-activity` consulta persistente e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço de publicação AEM e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
