---
title: Tutorial prático da introdução ao AEM Headless - GraphQL
description: Um tutorial completo que ilustra como criar e expor conteúdo usando as APIs do AEM GraphQL.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
duration: 54
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 4%

---

# Introdução ao AEM Headless - GraphQL

Um tutorial completo que ilustra como criar e expor conteúdo usando as APIs do GraphQL do AEM e consumi-lo por um aplicativo externo, em um cenário de CMS headless.

Este tutorial explora como as APIs GraphQL do AEM e os recursos headless podem ser usados para potencializar as experiências encontradas em um aplicativo externo.

Este tutorial aborda os seguintes tópicos:

* Criar uma configuração de projeto
* Criar modelos de fragmento de conteúdo para modelar dados
* Crie fragmentos de conteúdo com base nos modelos criados anteriormente.
* Explore como os fragmentos de conteúdo no AEM podem ser consultados usando a ferramenta de desenvolvimento GraphiQL integrada.
* Para armazenar ou criar consultas persistentes do GraphQL no AEM
* Consumir consultas persistentes do GraphQL de um aplicativo React de amostra

## Pré-requisitos {#prerequisites}

São necessários os seguintes itens para seguir este tutorial:

* Habilidades básicas em HTML e JavaScript
* As seguintes ferramentas devem ser instaladas localmente:
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Um IDE (por exemplo, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Ambiente do AEM

Para concluir este tutorial, é recomendável ter acesso de administrador do AEM a um ambiente do AEM as a Cloud Service. Se você não tiver acesso a um ambiente do AEM as a Cloud Service, [inscreva-se na versão de avaliação do AEM Headless](https://commerce.adobe.com/business-trial/sign-up?items%5B0%5D%5Bid%5D=649A1AF5CBC5467A25E84F2561274821&amp;cli=headless_exl_banner_campaign&amp;co=US&amp;lang=en) para explorar os recursos headless do AEM.

## Vamos começar!

Inicie o tutorial com [Definindo Modelos de Fragmento de Conteúdo](content-fragment-models.md).

## Projeto do GitHub

O código-fonte e os pacotes de conteúdo estão disponíveis no [AEM Guides - Projeto GitHub do WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql).

Se você encontrar um problema com o tutorial ou o código, deixe um [problema do GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).
