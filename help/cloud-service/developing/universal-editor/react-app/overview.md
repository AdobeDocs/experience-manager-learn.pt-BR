---
title: Reagir à edição do aplicativo usando o Editor universal
description: Saiba como editar o conteúdo de uma amostra do aplicativo React usando o Editor universal.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Reagir à edição do aplicativo usando o Editor universal

Saiba como editar o conteúdo de uma amostra do aplicativo React usando o Editor universal. O conteúdo é armazenado nos Fragmentos de conteúdo no AEM e é buscado usando as APIs do GraphQL.

Este tutorial o orienta pelo processo de configuração do ambiente de desenvolvimento local, instrumentação do aplicativo React para editar seu conteúdo e edição do conteúdo usando o Editor universal.

## O que você aprende

Este tutorial aborda os seguintes tópicos:

- Uma breve visão geral do Universal Editor
- Configuração do ambiente de desenvolvimento local
   - **SDK do AEM**: fornece o conteúdo armazenado nos Fragmentos de conteúdo para o aplicativo React usando APIs do GraphQL.
   - **aplicativo React**: uma interface simples que exibe o conteúdo do AEM.
   - **Serviço de Editor Universal**: a _cópia local do serviço Editor Universal_ que vincula o Editor universal e o SDK do AEM.
- Como instrumentar o aplicativo React para editar o conteúdo usando o Editor universal
- Como editar o conteúdo do aplicativo React usando o Editor universal


## Visão geral do Universal Editor

O Editor universal capacita os autores e desenvolvedores de conteúdo (front-end e back-end). Vamos analisar alguns dos principais benefícios do Editor universal:

- Criado para editar conteúdo headless e headful no modo what-you-see-is-what-you-get (WYSIWYG).
- Fornece uma experiência consistente de edição de conteúdo em diferentes tecnologias de front-end, como React, Angular, Vue etc. Dessa forma, os autores de conteúdo podem editar o conteúdo sem se preocuparem com a tecnologia front-end subjacente.
- É necessária instrumentação mínima para habilitar o Editor Universal no aplicativo front-end. Dessa forma, maximiza a produtividade do desenvolvedor e os libera para se concentrarem na criação da experiência.
- Separação de preocupações em três funções, autores de conteúdo, desenvolvedores de front-end e desenvolvedores de back-end, permitindo que cada função se concentre em suas responsabilidades principais.


## Amostra do aplicativo React

Este tutorial usa [**Equipes da WKND**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) como a amostra do aplicativo React. A variável **Equipes da WKND** O aplicativo React exibe uma lista de membros da equipe e seus detalhes.

Os detalhes da Equipe, como título, descrição e membros da equipe, são armazenados como _Equipe_ Fragmentos de conteúdo no AEM. Da mesma forma, os detalhes da Pessoa, como nome, biografia e foto do perfil, são armazenados como _Person_ Fragmentos de conteúdo no AEM.

O conteúdo do aplicativo React é fornecido por AEM usando APIs do GraphQL e a interface do usuário é criada usando dois componentes React, `Teams` e `Person`.

Um tutorial correspondente está disponível para saber como criar a **Equipes da WKND** aplicativo React. Você pode encontrar o tutorial [aqui](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Próxima etapa

Saiba como [configurar o ambiente de desenvolvimento local](./local-development-setup.md).