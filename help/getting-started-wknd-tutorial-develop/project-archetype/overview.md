---
title: Introdução ao AEM Sites - Arquétipo de projeto
description: Introdução ao AEM Sites - Arquétipo de projeto. O tutorial WKND é um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager. O tutorial aborda a implementação de um site de AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais como configuração de projeto, arquétipos de maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
index: y
feature: Componentes principais, Editor de páginas, Modelos editáveis, Arquétipo de projeto AEM
topic: Gerenciamento de conteúdo, desenvolvimento
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 20%

---


# Introdução ao AEM Sites - Arquétipo de projeto {#project-archetype}

Bem-vindo a um tutorial de várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site de AEM para uma marca fictícia de estilo de vida na WKND.

Este tutorial é iniciado usando o [AEM Arquétipo de projeto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) para gerar um novo projeto.

O tutorial foi projetado para funcionar com **AEM como um Cloud Service** e é compatível com versões anteriores com **AEM 6.5.0+** e **AEM 6.4.8.1+**. O site é implementado usando:

* [Arquétipo de projeto AEM Maven](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos sling
* [Modelos editáveis](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema de estilos](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Estime de 1 a 2 horas para visualizar cada parte do tutorial.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeo são capturados usando o AEM como um Cloud Service SDK em execução em um ambiente Mac OS com o [Visual Studio Code](https://code.visualstudio.com/) como o IDE. Os comandos e o código devem ser independentes do sistema operacional local, salvo indicação em contrário.

### Software necessário

Devem ser instalados:

* Instância de AEM local **Autor** (Cloud Service SDK, 6.5.5+ ou 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/)  (LTS - Suporte a longo prazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Code ou IDE equivalente
   * [Sincronização de AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - Ferramenta usada em todo o tutorial

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o guia a  [seguir para configurar um ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desenvolvimento local.

## Github {#github}

Todo o código para o projeto pode ser encontrado no Github no AEM Guide repo:

**[GitHub: Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd)**

Além disso, cada parte do tutorial tem sua própria ramificação no GitHub. Um usuário pode iniciar o tutorial a qualquer momento, fazendo o check-out da ramificação que corresponde à parte anterior.

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o capítulo [Configuração do projeto](project-setup.md) e saiba como gerar um novo projeto do Adobe Experience Manager usando o Arquétipo de projeto AEM.
