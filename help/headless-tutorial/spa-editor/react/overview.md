---
title: Introdução ao Editor de SPA no AEM e React
description: Crie o seu primeiro aplicativo de página única (SPA) editável em React no Adobe Experience Manager (AEM) com o WKND SPA. Saiba como criar um SPA usando a estrutura React JS com o editor de SPA do AEM. Este tutorial de várias partes aborda a implementação de um aplicativo em React para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda a criação completa do SPA e a integração com o AEM.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 33%

---

# Criar o seu primeiro SPA React no AEM {#overview}

{{edge-delivery-services}}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novos no **Editor de SPA** recurso no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo React para uma marca fictícia de estilo de vida, a WKND. AEM O aplicativo React é desenvolvido e projetado para ser implantado com o editor SPA, que mapeia componentes do React para componentes do AEM. O SPA completo, implantado no AEM, pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementação do WKND SPA*

## Sobre

O tutorial foi projetado para funcionar com **AEM as a Cloud Service** e é compatível com versões anteriores com **AEM 6.5.4+** e **AEM 6.4.8+**.

## Código mais recente

Todo o código do tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A variável [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) O está disponível como Pacotes AEM para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Conhecimento básico do HTML, CSS e JavaScript
* Familiaridade básica com o [React](https://reactjs.org/tutorial/tutorial.html)

*Embora não seja obrigatório, é benéfico ter uma compreensão [desenvolvimento de componentes tradicionais do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR).*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são capturados usando o SDK as a Cloud Service do AEM em execução em um ambiente de sistema operacional Mac com [Código do Visual Studio](https://code.visualstudio.com/) como o IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

### Software necessário

* [SDK AS A CLOUD SERVICE AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) ou [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) e [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).
>
> **Novo no AEM 6.5?** Confira o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o [Criar projeto](create-project.md) e saiba como gerar um projeto habilitado para o Editor de SPA usando o Arquétipo de projeto AEM.
