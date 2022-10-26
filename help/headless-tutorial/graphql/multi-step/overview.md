---
title: Introdução ao AEM tutorial autônomo - GraphQL
description: Um tutorial completo que ilustra como criar e expor conteúdo usando APIs GraphQL AEM.
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 4%

---

# Introdução ao AEM Headless - GraphQL

Um tutorial completo que ilustra como criar e expor conteúdo usando APIs GraphQL AEM consumidas por um aplicativo externo, em um cenário de CMS sem periféricos.

Este tutorial explica como AEM APIs GraphQL e recursos sem periféricos podem ser usados para potencializar as experiências encontradas em um aplicativo externo.

Este tutorial aborda os seguintes tópicos:

* Criar uma configuração de projeto
* Criar modelos de fragmento de conteúdo para modelar dados
* Crie Fragmentos de conteúdo com base nos modelos feitos anteriormente.
* Explore como os Fragmentos de conteúdo no AEM podem ser consultados usando a ferramenta de desenvolvimento GraphiQL integrada.
* Para armazenar ou manter as consultas GraphQL no AEM
* Consumir consultas GraphQL persistentes de um aplicativo React de amostra


## Pré-requisitos {#prerequisites}

Os seguintes itens são necessários para seguir este tutorial:

* Habilidades básicas de HTML e JavaScript
* As seguintes ferramentas devem ser instaladas localmente:
   * [Node.js v14+](https://nodejs.org/en/)
   * [npm 6+](https://www.npmjs.com/)
   * [Git](https://git-scm.com/)
   * Um IDE (por exemplo, [Código Microsoft® Visual Studio](https://code.visualstudio.com/))

### Ambiente AEM

Para concluir este tutorial, AEM é recomendado o acesso do Administrador a um ambiente as a Cloud Service AEM. Se você não tiver acesso a AEM ambiente as a Cloud Service, poderá usar a variável [SDK do Quickstart local AEM as a Cloud Service](/help/cloud-service/local-development-environment/aem-runtime.md). No entanto, é importante observar que algumas telas da interface do usuário do produto, como a navegação do Fragmento de conteúdo, são diferentes.

## Vamos começar!

Inicie o tutorial com [Definição dos modelos de fragmento do conteúdo](content-fragment-models.md).

## Projeto do GitHub

O código-fonte e os pacotes de conteúdo estão disponíveis na variável [Guias de AEM - Projeto GitHub GraphQL da WKND](https://github.com/adobe/aem-guides-wknd-graphql).

Se você encontrar um problema com o tutorial ou o código, deixe um [Problema do GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).
