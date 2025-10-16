---
title: 'Introdução ao AEM Sites: arquétipo de projeto'
description: 'Introdução ao AEM Sites: arquétipo de projeto. O tutorial da WKND é um tutorial em várias partes projetado para desenvolvedores novatos no Adobe Experience Manager. Este tutorial aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais, como configuração de projetos, arquétipos do Maven, componentes principais, modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.'
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: display, noCatalog
duration: 74
source-git-commit: b612e2e36af8f56661a07577e979959c650564ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 100%

---

# Introdução ao AEM Sites: arquétipo de projeto {#project-archetype}

{{traditional-aem}}

Boas-vindas a esse tutorial em várias partes projetado para desenvolvedores iniciantes no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND.

Este tutorial começa com o uso do [Arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) para gerar um novo projeto.

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e conta com compatibilidade retroativa com o **AEM 6.5.14+**. O site é implementado usando:

* [Arquétipo de projeto do AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR)
* [Componentes principais](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=pt-BR)
* [Modelos sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modelos editáveis](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=pt-BR)
* [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=pt-BR)

*Estimativa de 1 a 2 horas para passar por cada parte do tutorial.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são captados por meio do SDK do AEM as a Cloud Service em execução em um ambiente macOS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

### Software necessário

Devem ser instalados:

* [Instância de **Criação** do AEM local](https://experience.adobe.com/#/downloads) (SDK do Cloud Service ou 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/pt) (LTS, compatibilidade de longo prazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) ou IDE equivalente
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync): ferramenta usada em todo o tutorial

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).
>
> **Primeira vez usando o AEM 6.5?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## GitHub {#github}

O código deste tutorial pode ser encontrado no GitHub, no repositório do Guia do AEM:

**[GitHub: projeto de sites da WKND](https://github.com/adobe/aem-guides-wknd)**

Além disso, cada parte do tutorial tem sua própria ramificação no GitHub. Um usuário pode começar o tutorial a qualquer momento, bastando consultar a ramificação que corresponde à parte anterior.

## Próximas etapas {#next-steps}

O que você está esperando? Para iniciar o tutorial, navegue até o capítulo [Configuração do projeto](project-setup.md) e saiba como gerar um novo projeto do Adobe Experience Manager com o arquétipo de projeto do AEM.
