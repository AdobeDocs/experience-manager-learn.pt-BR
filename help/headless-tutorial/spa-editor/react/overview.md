---
title: Introdução ao editor de SPA no AEM e React
description: Crie o seu primeiro aplicativo de página única (SPA) editável em React no Adobe Experience Manager (AEM) com o WKND SPA. Saiba como criar um SPA usando a estrutura React JS com o editor de SPA do AEM. Este tutorial de várias partes aborda a implementação de um aplicativo em React para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda a criação completa do SPA e a integração com o AEM.
version: Experience Manager as a Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 71
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '417'
ht-degree: 100%

---

# Crie o seu primeiro SPA do React no AEM {#overview}

{{edge-delivery-services}}

Boas-vindas a esse tutorial de várias partes, projetado para iniciantes no desenvolvimento com o recurso do **Editor de SPA** do Adobe Experience Manager (AEM). Este tutorial em várias partes aborda a implementação de um aplicativo em React para uma marca fictícia de estilo de vida, a WKND. O aplicativo em React foi desenvolvido e projetado para ser implantado com o editor de SPA do AEM, que mapeia componentes em React para componentes do AEM. O SPA concluído e implantado no AEM pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementação do SPA da WKND*

## Sobre

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e tem compatibilidade retroativa com o **AEM 6.5.4+** e o **AEM 6.4.8+**.

## Código mais recente

Todo o código do tutorial pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponível como pacotes do AEM para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Conhecimento básico de HTML, CSS e JavaScript
* Familiaridade básica com [React](https://reactjs.org/tutorial/tutorial.html)

*Embora não seja obrigatório, é vantajoso ter uma compreensão básica sobre o [desenvolvimento de componentes tradicionais de sites do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR).*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são captados por meio do SDK do AEM as a Cloud Service em execução em um ambiente macOS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

### Software necessário

* [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime), [AEM 6.5.4+](https://experienceleague.adobe.com/pt-br/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates#aem-65) ou [AEM 6.4.8+](https://experienceleague.adobe.com/pt-br/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/pt) e [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Primeira vez usando o AEM 6.5?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## Próximas etapas {#next-steps}

O que você está esperando? Para começar o tutorial, navegue até o capítulo [Criar projeto](create-project.md) e saiba como gerar um projeto habilitado para o editor de SPA usando o arquétipo de projeto do AEM.
