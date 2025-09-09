---
title: Tutorial da OpenAPI do AEM Headless | Entrega de fragmento de conteúdo
description: Um tutorial completo que ilustra como criar e expor conteúdo usando as APIs de entrega de fragmentos de conteúdo baseadas em OpenAPI do AEM.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 22%

---

# Introdução à entrega de fragmentos de conteúdo do AEM com APIs OpenAPI

Explore este tutorial que ilustra como criar e expor conteúdo do AEM usando a entrega de fragmento de conteúdo do AEM com APIs OpenAPI e consumido por um aplicativo externo, em um cenário de CMS headless. Isso explora esses conceitos pensando na criação de um aplicativo React que exibe equipes WKND e detalhes associados dos membros. As equipes e os membros são modelados usando modelos de fragmento de conteúdo do AEM e consumidos pelo aplicativo React usando a entrega de fragmentos de conteúdo do AEM com APIs OpenAPI.

![Aplicativo de equipes do WKND](./assets/overview/main.png)

Este tutorial aborda os seguintes tópicos:

* Criar uma configuração de projeto
* Criar modelos de fragmento de conteúdo para modelar dados
* Criar fragmentos de conteúdo com base nos modelos criados anteriormente
* Saiba como os fragmentos de conteúdo no AEM podem ser consultados usando o recurso &quot;Experimente&quot; da documentação do AEM Content Fragment Delivery with OpenAPIs
* Consumir dados de Fragmento de conteúdo por meio da entrega de Fragmento de conteúdo do AEM com chamadas da API OpenAPI de uma amostra do aplicativo React
* Aprimorar o aplicativo React para ser editável no Editor universal

## Pré-requisitos {#prerequisites}

Os seguintes elementos são necessários para seguir este tutorial:

* AEM Sites as a Cloud Service
* Habilidades básicas em HTML e JavaScript
* As seguintes ferramentas devem ser instaladas localmente:
   * [Node.js v22+](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Um IDE (por exemplo, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Ambiente do AEM as a Cloud Service

Para concluir este tutorial, é recomendável que você tenha acesso de **Administrador do AEM** a um ambiente do AEM as a Cloud Service. Também é possível usar um ambiente **Desenvolvimento**, **Ambiente de Desenvolvimento Rápido** ou um ambiente em um **Programa de Sandbox**.

## Vamos começar.

Inicie o tutorial com o tema [Definir modelos de fragmento de conteúdo](1-content-fragment-models.md).

## Projeto do GitHub

O código-fonte e os pacotes de conteúdo estão disponíveis no [repositório GitHub dos tutoriais do AEM Headless](https://github.com/adobe/aem-tutorials).

A ramificação [`main` contém o código-fonte final ](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) para este tutorial.
Instantâneos do código ao final de cada etapa estão disponíveis como tags Git.

* Início do capítulo 4 - Aplicativo React: [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Fim do capítulo 4 - Aplicativo React: [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Fim do capítulo 5 - Editor Universal: [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Se você encontrar um problema com o tutorial ou o código, abra um [problema do GitHub](https://github.com/adobe/aem-tutorials/issues).
