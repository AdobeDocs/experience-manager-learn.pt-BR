---
title: Introdução ao Editor e Angular de SPA do AEM
description: Crie seu primeiro Aplicativo de página única (SPA) angular que seja editável no Adobe Experience Manager, AEM com o SPA da WKND. Saiba como criar um SPA usando a estrutura Angular JS com o Editor SPA do AEM. Este tutorial em várias partes aborda a implementação de um aplicativo Angular para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda a criação completa do SPA e a integração com o AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 14%

---


# Crie seu primeiro SPA Angular no AEM {#introduction}

Bem-vindo a um tutorial de várias partes projetado para desenvolvedores novos no recurso **Editor SPA** no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo Angular para uma marca fictícia de estilo de vida, a WKND. O aplicativo Angular será desenvolvido e projetado para ser implantado com o Editor de SPA do AEM, que mapeia componentes Angulares em componentes do AEM. O SPA concluído, implantado no AEM, pode ser criado dinamicamente com as ferramentas de edição online tradicionais do AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementação de SPA WKND*

## Sobre

O objetivo deste tutorial de várias partes é ensinar um desenvolvedor a implementar um aplicativo Angular para trabalhar com o recurso Editor do SPA do AEM. Em um cenário real, as atividades de desenvolvimento são divididas por persona, geralmente envolvendo um **desenvolvedor do Front-End** e um **desenvolvedor do Back-End**. Acreditamos que é útil para qualquer desenvolvedor que esteja envolvido em um projeto do AEM SPA Editor concluir este tutorial.

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e é compatível com versões anteriores do **AEM 6.5.4+** e **AEM 6.4.8+**. O SPA é implementado usando:

* [Arquétipo de projeto do AEM Maven](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor SPA do AEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*Estime de 1 a 2 horas para visualizar cada parte do tutorial.*

## Código mais recente

Todo o código tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponível como pacotes AEM que podem ser baixados.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Um conhecimento básico de HTML, CSS e JavaScript
* Familiaridade básica com [Angular](https://angular.io/)
* [SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) do AEM as a Cloud Service,  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) ou  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.](https://nodejs.org/en/) jand  [npm](https://www.npmjs.com/)

*Embora não seja obrigatório, é benéfico ter uma compreensão básica do  [desenvolvimento dos componentes](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) tradicionais do AEM Sites.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeo são capturados usando o SDK do AEM as a Cloud Service em execução em um ambiente Mac OS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, salvo indicação em contrário.

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o guia a  [seguir para configurar um ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desenvolvimento local.

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o capítulo [SPA Editor Project](create-project.md) e saiba como gerar um projeto habilitado para o SPA Editor usando o Arquétipo de projeto do AEM.

## Compatibilidade com versões anteriores {#compatibility}

O código do projeto deste tutorial foi criado para o AEM as a Cloud Service. Para tornar o código do projeto compatível retroativamente para **6.5.4+** e **6.4.8+**, várias modificações foram feitas.

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

Um perfil Maven adicional, chamado `classic`, foi adicionado para modificar a build para direcionar os ambientes AEM 6.x:

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

O perfil `classic` é desativado por padrão. Se seguir o tutorial com o AEM 6.x, adicione o perfil `classic` sempre que instruído a executar uma compilação Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao gerar um novo projeto para uma implementação do AEM, sempre use a versão mais recente do [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) e atualize o `aemVersion` para direcionar sua versão pretendida do AEM.
