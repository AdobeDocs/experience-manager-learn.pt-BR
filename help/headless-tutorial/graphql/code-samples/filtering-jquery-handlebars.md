---
title: Filtrando com jQuery e Handlebars
description: Uma implementação do JavaScript usando jQuery e Handlebars que filtra as Aventuras WKND para exibição. .
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
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Filtrando com jQuery e Handlebars

Explore a capacidade das APIs do GraphQL sem periféricos do AEM de filtrar dados usando um aplicativo do JavaScript que usa o [jQuery](https://jquery.com/) e o [Handlebars](https://handlebarsjs.com/). Este aplicativo cria uma lista de Aventuras WKND filtráveis por Tipo de atividade.

Este código demonstra o uso de Adobe [AEM Headless Client para JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) para invocar consultas persistentes do GraphQL. Este aplicativo usa a consulta persistente `wknd-shared/adventures-all` para coletar todas as aventuras e obter uma lista de Tipos de atividade disponíveis. Quando um usuário seleciona um Tipo de atividade, o tipo selecionado é passado para a consulta persistente `wknd-shared/adventures-by-activity` e recupera os detalhes de aventura somente para as aventuras do Tipo de atividade especificado.

Este código:

+ Conecta-se a um serviço AEM Publish e não requer autenticação
+ Usa as consultas persistentes do WKND: `wknd-shared/adventures-all` e `wknd-shared/adventures-by-activity`
