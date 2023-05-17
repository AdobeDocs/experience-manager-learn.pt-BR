---
title: Primeiro tutorial autônomo do AEM
description: Saiba como ser um aplicativo AEM Headless First.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---


# Primeiro tutorial autônomo do AEM

![Primeiro tutorial autônomo do AEM](./assets/overview/overview.png)

Bem-vindo ao tutorial sobre a criação de uma experiência da Web usando o React, totalmente alimentado AEM APIs headless e GraphQL. Neste tutorial, vamos orientá-lo pelo processo de criação de uma aplicação web dinâmica e interativa ao combinar a eficiência do React, das APIs headless do Adobe Experience Manager (AEM) e do GraphQL.

O React é uma biblioteca JavaScript popular para criar interfaces de usuário, conhecida por sua simplicidade, reutilização e arquitetura baseada em componentes. O AEM oferece recursos robustos de gerenciamento de conteúdo e expõe APIs sem periféricos que permitem aos desenvolvedores acessar o conteúdo e os dados armazenados no AEM por meio de uma variedade de canais e aplicativos.

Ao aproveitar AEM APIs headless, você pode recuperar conteúdo, ativos e dados da instância do AEM e usá-los para potencializar o aplicativo React. O GraphQL, uma linguagem de consulta flexível para APIs, fornece uma maneira eficiente e precisa de solicitar dados específicos da sua instância do AEM, permitindo uma integração perfeita entre o React e o AEM.

Ao longo deste tutorial, vamos orientá-lo pelo processo passo a passo de criação de uma experiência da Web usando o React e AEM APIs headless com o GraphQL. Você aprenderá a configurar seu ambiente de desenvolvimento, estabelecer uma conexão entre o React e o AEM, recuperar conteúdo usando consultas do GraphQL e renderizá-lo dinamicamente em seu aplicativo da Web.

Abordaremos tópicos como a configuração do seu projeto React, estabelecimento de autenticação com AEM, consulta de conteúdo de AEM usando o GraphQL, manipulação de dados em seus componentes React e otimização de desempenho utilizando armazenamento em cache e paginação.

Ao final deste tutorial, você terá uma sólida compreensão de como aproveitar o React, AEM APIs headless e o GraphQL para criar uma experiência online poderosa e envolvente. Então, vamos mergulhar e começar a criar sua próxima aplicação web!

## Pré-requisitos

### Habilidades

+ Proficiência no React
+ Proficiência no GraphQL
+ Conhecimento básico AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requer o acesso do Administrador a um ambiente AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/en/)
   + Verifique a versão do nó executando `node -v` na linha de comando
+ [npm 6+](https://www.npmjs.com/)
   + Verifique sua versão do npm executando `npm -v` na linha de comando
+ [Git](https://git-scm.com/)
   + Verifique a versão do Git executando `git -v` na linha de comando

Use [gerenciador de versão do nó (nvm)](https://github.com/nvm-sh/nvm) para resolver ter várias versões do node.js na mesma máquina.

Assegure-se de ter privilégios para instalar software globalmente em seu computador.

## Próxima etapa

Agora que seu ambiente foi configurado, vamos para a próxima etapa: [Configurar e criar conteúdo em AEM as a Cloud Service](./1-content-modeling.md)
