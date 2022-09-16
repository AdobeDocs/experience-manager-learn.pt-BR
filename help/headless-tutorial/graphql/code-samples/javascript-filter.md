---
title: Filtro de consultas
description: Uma implementação do JavaScript que permite selecionar Fragmentos de conteúdo específicos e, em seguida, exibir seus detalhes .
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
source-git-commit: 680ed62141b853daf104a827067ca6d5a209796d
workflow-type: tm+mt
source-wordcount: '57'
ht-degree: 3%

---


# Filtro de consultas

Este exemplo de JavaScript e Handlebars ilustra como filtrar os resultados de GraphQL e exibir os resultados selecionados.

Este código:

+ Conecta a [wknd.site](https://wknd.site)Serviço de publicação do AEM do e não requer autenticação
+ Usa as consultas persistentes: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-slug`
