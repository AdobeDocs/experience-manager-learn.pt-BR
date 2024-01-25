---
title: Primeiro tutorial sobre AEM Headless
description: Saiba como ser um aplicativo AEM Headless primeiro.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 99
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---

# Primeiro tutorial sobre AEM Headless

{{aem-headless-trials-promo}}

Bem-vindo ao tutorial sobre como criar uma experiência da Web usando o React, totalmente equipado com APIs AEM Headless e o GraphQL. Neste tutorial, guiaremos você pelo processo de criação de uma aplicação Web dinâmica e interativa combinando o poder das APIs headless do React, do Adobe Experience Manager (AEM) e do GraphQL.

O React é uma biblioteca popular de JavaScript para a construção de interfaces de usuário, conhecida por sua simplicidade, reutilização e arquitetura baseada em componentes. O AEM fornece recursos robustos de gerenciamento de conteúdo e expõe APIs headless que permitem aos desenvolvedores acessar conteúdo e dados armazenados no AEM por meio de uma variedade de canais e aplicativos.

Ao utilizar APIs AEM Headless, você pode recuperar conteúdo, ativos e dados da instância do AEM e usá-los para potencializar o aplicativo React. O GraphQL, uma linguagem de consulta flexível para APIs, fornece uma maneira eficiente e precisa de solicitar dados específicos da sua instância do AEM, permitindo uma integração perfeita entre o React e o AEM.

![Primeiro tutorial sobre AEM Headless](./assets/overview/overview.png)

Neste tutorial, guiaremos você pelo processo passo a passo de criação de uma experiência da Web usando as APIs do React e do AEM Headless com o GraphQL. Você aprenderá a configurar seu ambiente de desenvolvimento, estabelecer uma conexão entre o React e o AEM, recuperar conteúdo usando consultas do GraphQL e renderizá-lo dinamicamente em seu aplicativo web.

Abordaremos tópicos como configurar seu projeto no React, estabelecer autenticação com AEM, consultar conteúdo do AEM usando o GraphQL, manipular dados em seus componentes do React e otimizar o desempenho usando armazenamento em cache e paginação.

Ao final deste tutorial, você terá uma sólida compreensão de como aproveitar as APIs do React, do AEM Headless e do GraphQL para criar uma experiência da Web poderosa e envolvente. Então, vamos nos aprofundar e começar a criar seu próximo aplicativo web!

## Pré-requisitos

### Habilidades

+ Proficiência no React
+ Proficiência no GraphQL
+ Conhecimento básico do AEM as a Cloud Service

### AEM as a Cloud Service

Este tutorial requer acesso de administrador a um ambiente as a Cloud Service do AEM.

### Software

+ [Node.js v16+](https://nodejs.org/en/)
   + Verifique a versão do nó executando `node -v` da linha de comando
+ [npm 6+](https://www.npmjs.com/)
   + Verifique sua versão npm executando `npm -v` da linha de comando
+ [Git](https://git-scm.com/)
   + Verifique sua versão do Git executando `git -v` da linha de comando

Uso [gerenciador de versão de nó (nvm)](https://github.com/nvm-sh/nvm) para tratar de ter várias versões de node.js na mesma máquina.

Verifique se você tem privilégios para instalar o software globalmente no computador.

## Próxima etapa

Agora que seu ambiente está configurado, vamos para a próxima etapa: [Configuração e criação de conteúdo no AEM as a Cloud Service](./1-content-modeling.md)
