---
title: Filtragem com jQuery e Handlebars
description: Uma implementação do JavaScript usando jQuery e Handlebars que filtra Aventuras WKND para exibição. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---


# Filtragem com jQuery e Handlebars

Explore AEM capacidade das APIs GraphQL sem cabeçalho para filtrar dados usando um aplicativo JavaScript que usa [jQuery](https://jquery.com/) e [Handlebars](https://handlebarsjs.com/). Este aplicativo cria uma lista de aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso do Adobe [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para chamar consultas GraphQL persistentes. Este aplicativo usa a variável `wknd-shared/adventures-all` consulta persistente para coletar todas as aventuras e derivar uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a função `wknd-shared/adventures-by-activity` consulta persistente e recupera os detalhes da aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço de publicação do AEM e não requer autenticação
+ Usa as consultas persistentes de WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
