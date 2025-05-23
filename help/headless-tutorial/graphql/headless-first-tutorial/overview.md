---
title: Primeiro tutorial do AEM Headless
description: Saiba como ser um primeiro aplicativo headless do AEM.
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
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---

# Primeiro tutorial do AEM Headless

Bem-vindo ao tutorial sobre como criar uma experiência da Web usando o React, totalmente habilitado pelas APIs do AEM Headless e pelo GraphQL. Neste tutorial, guiaremos você pelo processo de criação de uma aplicação web dinâmica e interativa combinando o poder do React, das APIs headless do Adobe Experience Manager (AEM) e do GraphQL.

O React é uma biblioteca popular do JavaScript para criação de interfaces de usuário, conhecida por sua simplicidade, reutilização e arquitetura baseada em componentes. O AEM fornece recursos robustos de gerenciamento de conteúdo e expõe APIs headless que permitem aos desenvolvedores acessar conteúdo e dados armazenados no AEM por meio de uma variedade de canais e aplicativos.

Ao usar as APIs headless do AEM, você pode recuperar conteúdo, ativos e dados da instância do AEM e usá-los para potencializar o aplicativo React. O GraphQL, uma linguagem de consulta flexível para APIs, fornece uma maneira eficiente e precisa de solicitar dados específicos da sua instância do AEM, permitindo uma integração perfeita entre o React e o AEM.

![Primeiro tutorial sobre o AEM Headless](./assets/overview/overview.png)

Neste tutorial, guiaremos você pelo processo passo a passo de criação de uma experiência da Web usando as APIs do React e do AEM Headless com o GraphQL. Você aprenderá a configurar seu ambiente de desenvolvimento, estabelecer uma conexão entre o React e o AEM, recuperar conteúdo usando consultas do GraphQL e renderizá-lo dinamicamente em seu aplicativo web.

Abordaremos tópicos como configurar seu projeto no React, estabelecer autenticação com o AEM, consultar conteúdo do AEM usando o GraphQL, manipular dados em seus componentes do React e otimizar o desempenho usando armazenamento em cache e paginação.

Ao final deste tutorial, você terá uma sólida compreensão de como aproveitar o React, as APIs do AEM Headless e o GraphQL para criar uma experiência da Web poderosa e envolvente. Então, vamos nos aprofundar e começar a criar seu próximo aplicativo web!

## Pré-requisitos

### Habilidades

+ Proficiência no React
+ Proficiência no GraphQL
+ Conhecimento básico do AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requer acesso de administrador a um ambiente do AEM as a Cloud Service.

### Software

+ [Node.js v16+](https://nodejs.org/en/)
   + Verifique a versão do nó executando `node -v` na linha de comando
+ [npm 6+](https://www.npmjs.com/)
   + Verifique sua versão do npm executando `npm -v` na linha de comando
+ [Git](https://git-scm.com/)
   + Verifique sua versão do Git executando `git -v` na linha de comando

Use o [gerenciador de versão de nó (nvm)](https://github.com/nvm-sh/nvm) para endereçar ter várias versões do node.js na mesma máquina.

Verifique se você tem privilégios para instalar o software globalmente no computador.

## Próxima etapa

Agora que seu ambiente está configurado, vamos seguir para a próxima etapa: [Configurar e criar conteúdo no AEM as a Cloud Service](./1-content-modeling.md)
