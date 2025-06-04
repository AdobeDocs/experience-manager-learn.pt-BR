---
title: Tutorial do AEM com foco em headless
description: Saiba como criar um aplicativo do AEM com foco em headless.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '421'
ht-degree: 100%

---

# Tutorial do AEM com foco em headless

Boas-vindas ao tutorial de como criar uma experiência da web por meio do React, totalmente viabilizado pelas APIs do AEM Headless e pelo GraphQL. Neste tutorial, apresentaremos o processo de criação de um aplicativo web dinâmico e interativo, combinando os recursos do React, das APIs headless do Adobe Experience Manager (AEM) e do GraphQL.

O React é uma biblioteca popular de JavaScript para criação de interfaces de usuário, conhecida por sua simplicidade, reusabilidade e arquitetura baseada em componentes. O AEM fornece recursos robustos de gerenciamento de conteúdo e expõe APIs headless que permitem que os desenvolvedores acessem conteúdo e dados armazenados no AEM por meio de uma variedade de canais e aplicativos.

Por meio das APIs do AEM Headless, você pode recuperar conteúdo, ativos e dados da instância do AEM e usá-los para alimentar o aplicativo do React. O GraphQL, uma linguagem de consulta flexível para APIs, fornece uma maneira eficiente e precisa de solicitar dados específicos da sua instância do AEM, permitindo uma integração perfeita entre o React e o AEM.

![Tutorial do AEM com foco em headless](./assets/overview/overview.png)

Neste tutorial, apresentaremos o processo passo a passo para criação de uma experiência da web por meio do React e das APIs do AEM Headless com o GraphQL. Você aprenderá a configurar o seu ambiente de desenvolvimento, estabelecer uma conexão entre o React e o AEM, recuperar conteúdo por meio de consultas de GraphQL e renderizá-lo dinamicamente no seu aplicativo web.

Abordaremos tópicos como a configuração de projetos em React, estabelecimento da autenticação com o AEM, consulta de conteúdo do AEM por meio de GraphQL, manipulação de dados nos componentes do React e otimização do desempenho por meio do armazenamento em cache e da paginação.

Ao término deste tutorial, você terá uma boa compreensão de como aproveitar o React, as APIs do AEM Headless e o GraphQL para criar uma experiência da web avançada e envolvente. Então, vamos nos aprofundar e começar a criar o seu próximo aplicativo web.

## Pré-requisitos

### Habilidades

+ Proficiência em React
+ Proficiência no GraphQL
+ Conhecimento básico do AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requer acesso de admin a um ambiente do AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/pt)
   + Verifique a versão do nó executando `node -v` na linha de comando
+ [npm 6+](https://www.npmjs.com/)
   + Verifique a sua versão do npm, executando `npm -v` na linha de comando
+ [Git](https://git-scm.com/)
   + Verifique a sua versão do Git, executando `git -v` na linha de comando

Use o [gerenciador de versão de nó (nvm)](https://github.com/nvm-sh/nvm) para lidar com a existência de várias versões do node.js no mesmo computador.

Verifique se você tem privilégios para instalar o software globalmente no computador.

## Próxima etapa

Agora que o seu ambiente foi configurado, vamos seguir para a próxima etapa: [Configurar e criar conteúdo no AEM as a Cloud Service](./1-content-modeling.md)
