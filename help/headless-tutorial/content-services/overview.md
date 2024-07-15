---
title: Introdução ao AEM Headless - Serviços de conteúdo
description: Um tutorial completo que ilustra como criar e expor conteúdo usando o AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 4%

---

# Introdução ao AEM Headless - Serviços de conteúdo

Os Serviços de conteúdo do AEM usam as páginas AEM tradicionais para compor endpoints da API REST headless, e os componentes do AEM definem, ou fazem referência, ao conteúdo a ser exposto nesses endpoints.

O AEM Content Services permite as mesmas abstrações de conteúdo usadas para a criação de páginas da Web no AEM Sites, para definir o conteúdo e os esquemas dessas APIs HTTP. O uso de Páginas AEM e Componentes AEM permite que os profissionais de marketing componham e atualizem rapidamente APIs JSON flexíveis que podem potencializar qualquer aplicativo.

## Tutorial dos serviços de conteúdo

Um tutorial completo que ilustra como criar e expor conteúdo usando AEM e consumido por um aplicativo móvel nativo, em um cenário de CMS headless.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Este tutorial explica como os serviços de conteúdo do AEM podem ser usados para potencializar a experiência de um aplicativo móvel que exibe informações do evento (música, desempenho, arte, etc.) com curadoria da equipe da WKND.

Este tutorial abordará os seguintes tópicos:

* Criar conteúdo que representa um evento usando fragmentos de conteúdo
* Defina um ponto final do AEM Content Services usando Modelos e páginas do AEM Sites que expõem os dados do Evento como JSON
* Saiba como os Componentes principais do WCM no AEM podem ser usados para permitir que os profissionais de marketing criem pontos de extremidade JSON
* Consumir AEM Content Services JSON de um aplicativo móvel
   * O uso do Android é porque ele tem um emulador entre plataformas que todos os usuários (Windows, macOS e Linux) deste tutorial podem usar para executar o aplicativo nativo.

## Projeto do GitHub

O código-fonte e os pacotes de conteúdo estão disponíveis no [AEM Guides - Projeto GitHub do WKND Mobile](https://github.com/adobe/aem-guides-wknd-mobile).

Se você encontrar um problema com o tutorial ou o código, deixe um [problema do GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL versus AEM Content Services

|                                | APIs do GraphQL para AEM | Serviços de conteúdo AEM |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmentos do conteúdo estruturado | Componentes do AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes do AEM |
| Descoberta de conteúdo | Por consulta do GraphQL | Por página do AEM |
| Formato de entrega | GRAPHQL JSON | JSON do exportador do componente AEM |
