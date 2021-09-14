---
title: Introdução ao Editor e Angular de SPA do AEM
description: Crie seu primeiro Aplicativo de página única (SPA) do Angular que seja editável no Adobe Experience Manager, AEM com o SPA WKND. Saiba como criar um SPA usando a estrutura JS do Angular com AEM Editor de SPA. Este tutorial em várias partes aborda a implementação de um aplicativo Angular para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda a criação completa do SPA e a integração com o AEM.
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 10%

---

# Crie seu primeiro SPA de Angular em AEM {#introduction}

Bem-vindo a um tutorial de várias partes criado para desenvolvedores novos no recurso **SPA Editor** no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo Angular para uma marca fictícia de estilo de vida, a WKND. O aplicativo Angular será desenvolvido e projetado para ser implantado com AEM Editor de SPA, que mapeia os componentes do Angular para AEM componentes. O SPA concluído, implantado em AEM, pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA Final Implementado](assets/wknd-spa-implementation.png)

*Implementação de SPA WKND*

## Sobre

O objetivo deste tutorial em várias partes é ensinar um desenvolvedor a implementar um aplicativo do Angular para trabalhar com o recurso Editor de SPA do AEM. Em um cenário real, as atividades de desenvolvimento são divididas por persona, geralmente envolvendo um **desenvolvedor do Front-End** e um **desenvolvedor do Back-End**. Acreditamos que é útil para qualquer desenvolvedor que participará de um projeto AEM SPA Editor concluir este tutorial.

O tutorial foi projetado para funcionar com **AEM como um Cloud Service** e é compatível com versões anteriores com **AEM 6.5.4+** e **AEM 6.4.8+**. O SPA é implementado usando:

* [Arquétipo de projeto AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor de SPA AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Angular](https://angular.io/)

*Estime de 1 a 2 horas para visualizar cada parte do tutorial.*

## Código mais recente

Todo o código tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

O [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponível como Pacotes de AEM para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Um conhecimento básico de HTML, CSS e JavaScript
* Familiaridade básica com [Angular](https://angular.io/)
* [AEM como SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) do Cloud Service,  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) ou  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.](https://nodejs.org/en/) jand  [npm](https://www.npmjs.com/)

*Embora não seja obrigatório, é benéfico ter uma compreensão básica do  [desenvolvimento de componentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) AEM Sites tradicionais.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeo são capturados usando o AEM como um Cloud Service SDK em execução em um ambiente Mac OS com o [Visual Studio Code](https://code.visualstudio.com/) como o IDE. Os comandos e o código devem ser independentes do sistema operacional local, salvo indicação em contrário.

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o guia a  [seguir para configurar um ambiente](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desenvolvimento local.

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o capítulo [SPA Editor Project](create-project.md) e saiba como gerar um projeto habilitado para SPA Editor usando o Arquétipo de projeto AEM.

## Compatibilidade com versões anteriores {#compatibility}

O código do projeto deste tutorial foi criado para o AEM como um Cloud Service. Para tornar o código do projeto compatível retroativamente para **6.5.4+** e **6.4.8+**, várias modificações foram feitas.

O [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** foi incluído como uma dependência:

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

Um perfil Maven adicional, chamado `classic`, foi adicionado para modificar a build para direcionar AEM ambientes 6.x:

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

O perfil `classic` é desativado por padrão. Se seguir o tutorial com AEM 6.x, adicione o perfil `classic` sempre que instruído a executar uma build Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao gerar um novo projeto para uma implementação de AEM, sempre use a versão mais recente do [AEM Arquétipo de Projeto](https://github.com/adobe/aem-project-archetype) e atualize o `aemVersion` para direcionar sua versão pretendida do AEM.
