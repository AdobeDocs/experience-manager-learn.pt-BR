---
title: Introdução ao editor SPA AEM e reação
description: Crie seu primeiro aplicativo React Single Page (SPA) editável no Adobe Experience Manager AEM com o SPA WKND. Saiba como criar um SPA usando a estrutura React JS com AEM Editor SPA. Este tutorial em várias partes aborda a implementação de um aplicativo React para uma marca de estilo de vida fictício, a WKND. O tutorial aborda a criação completa do SPA e a integração com o AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 13%

---


# Crie seu primeiro SPA React em AEM {#overview}

Bem-vindo a um tutorial de várias partes projetado para desenvolvedores novos ao recurso Editor **** SPA no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo React para uma marca fictícia de estilo de vida, a WKND. O aplicativo React será desenvolvido e projetado para ser implantado com AEM Editor SPA, que mapeia componentes React para AEM componentes. O SPA concluído, implantado no AEM, pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementação do SPA WKND*

## Sobre

O objetivo deste tutorial multiparte é ensinar um desenvolvedor a implementar um aplicativo React para trabalhar com o recurso Editor SPA do AEM. Em um cenário real, as atividades de desenvolvimento são divididas por persona, geralmente envolvendo um desenvolvedor **** Front-End e um desenvolvedor **** Back-End. Acreditamos que é benéfico que qualquer desenvolvedor que esteja envolvido em um projeto do Editor SPA AEM conclua este tutorial.

O tutorial é projetado para funcionar com **AEM como um Cloud Service** e é compatível com versões anteriores **AEM 6.5.4+** e **AEM 6.4.8+**. O SPA é implementado usando:

* [Arquétipo de Projeto Maven AEM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor SPA AEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Reagir JS](https://reactjs.org/)
* [Criar aplicativo React](https://create-react-app.dev/)

*Estime de 1 a 2 horas para percorrer cada parte do tutorial.*

## Código mais recente

Todo o código do tutorial pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A base [de código](https://github.com/adobe/aem-guides-wknd-spa/releases) mais recente está disponível como Pacotes AEM disponíveis para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Um conhecimento básico sobre HTML, CSS e JavaScript
* Familiaridade básica com o [React](https://reactjs.org/tutorial/tutorial.html)
* [AEM como SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)Cloud Service, [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) ou [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) e [npm](https://www.npmjs.com/)

*Embora não seja obrigatório, é benéfico ter uma compreensão básica do[desenvolvimento de componentes](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)tradicionais da AEM Sites.*

## Ambiente de desenvolvimento local {#local-dev-environment}

É necessário um ambiente de desenvolvimento local para concluir este tutorial. Capturas de tela e vídeo são capturados usando o AEM como um SDK Cloud Service em execução em um ambiente Mac OS com código [do](https://code.visualstudio.com/) Visual Studio como IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que seja observado o contrário.

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o guia a [seguir para configurar um ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)de desenvolvimento local.

## Próximas etapas {#next-steps}

O que você está esperando?! Start o tutorial navegando até o capítulo Projeto [do Editor](create-project.md) SPA e saiba como gerar um projeto ativado pelo Editor SPA usando o AEM Project Archetype.

## Compatibilidade com versões anteriores {#compatibility}

O código do projeto deste tutorial foi criado para AEM como um Cloud Service. Para tornar o código do projeto compatível com versões anteriores para **6.5.4+** e **6.4.8+** , várias modificações foram feitas nos arquivos POM do tutorial.

O [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** foi incluído como uma dependência:

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

Um perfil Maven adicional, nomeado `classic` foi adicionado para modificar a compilação para o público alvo AEM ambientes 6.x:

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

O `classic` perfil é desativado por padrão. Se seguir o tutorial com AEM 6.x, adicione o `classic` perfil sempre que instruído a executar uma compilação Maven, isto é:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao gerar um novo projeto para uma implementação de AEM, sempre use a versão mais recente do [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) e atualize o `aemVersion` para público alvo da versão desejada do AEM.
