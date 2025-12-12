---
title: Editar aplicativo em React com o editor universal
description: Saiba como editar o conteúdo de um aplicativo em React de amostra com o editor universal.
short-description: Saiba como editar o conteúdo de um aplicativo em React de amostra com o editor universal. O conteúdo é armazenado nos fragmentos de conteúdo no AEM e obtido por meio das APIs do GraphQL.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 100%

---

# Editar aplicativo em React com o editor universal

Saiba como editar o conteúdo de um aplicativo em React de amostra com o editor universal. O conteúdo é armazenado nos fragmentos de conteúdo no AEM e obtido por meio das APIs do GraphQL.

Este tutorial apresenta o processo de configuração do ambiente de desenvolvimento local, a instrumentação do aplicativo em React para editar seu conteúdo e a edição do conteúdo com o editor universal.

## O que você aprenderá

Este tutorial aborda os seguintes tópicos:

- Uma visão geral breve do editor universal
- Configurar o ambiente de desenvolvimento local
   - **SDK do AEM**: fornece o conteúdo armazenado nos fragmentos de conteúdo do aplicativo em React por meio das APIs do GraphQL.
   - **Aplicativo em React**: uma interface do usuário simples que exibe o conteúdo a partir do AEM.
   - **Serviço do editor universal**: uma _cópia local do serviço do editor universal_ que vincula o editor universal ao SDK do AEM.
- Como instrumentar o aplicativo em React para editar o conteúdo com o editor universal
- Como editar o conteúdo do aplicativo em React com o editor universal


## Visão geral do editor universal

O editor universal capacita criadores e desenvolvedores de conteúdo (front-end e back-end). Vamos analisar alguns dos principais benefícios do editor universal:

- Criado para editar conteúdo com ou sem cabeçalho no modo “o que você vê é o que você leva” (WYSIWYG).
- Fornece uma experiência consistente de edição de conteúdo em diferentes tecnologias de front-end, como React, Angular, Vue etc. Dessa forma, os criadores de conteúdo podem editar o conteúdo sem se preocupar com a tecnologia de front-end subjacente.
- É necessária uma instrumentação mínima para habilitar o editor universal no aplicativo de front-end. Isso maximiza a produtividade do desenvolvedor e libera-o para concentrar-se na criação da experiência.
- Separação de áreas em três funções: criadores de conteúdo, desenvolvedores de front-end e desenvolvedores de back-end, permitindo que cada função se concentre em suas responsabilidades principais.


## Aplicativo em React de amostra

Este tutorial usa [**Equipes da WKND**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) como um aplicativo em React de amostra. O aplicativo em React **Equipes da WKND** exibe uma lista de membros da equipe e seus detalhes.

Os detalhes da equipe, como cargo, descrição e membros da equipe, são armazenados como fragmentos de conteúdo da _Equipe_ no AEM. Da mesma forma, os detalhes das pessoas, como nome, biografia e imagem de perfil, são armazenados como fragmentos de conteúdo de _Pessoa_ no AEM.

O conteúdo do aplicativo em React é fornecido pelo AEM por meio das APIs do GraphQL, e a interface do usuário é criada com base em dois componentes em React: `Teams` e `Person`.

Um tutorial correspondente está disponível para saber como criar o aplicativo em React **Equipes da WKND**. O tutorial pode ser encontrado [aqui](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Próxima etapa

Saiba como [configurar o ambiente de desenvolvimento local](./local-development-setup.md).
