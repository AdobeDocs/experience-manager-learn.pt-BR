---
title: Introdução ao Editor e Angular de SPA do AEM
description: Criar o seu primeiro aplicativo de página única (SPA) Angular que seja editável no Adobe Experience Manager (AEM), com o WKND SPA.
version: Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 14%

---

# Criar seu primeiro Angular SPA no AEM {#introduction}

{{edge-delivery-services}}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novos no **Editor de SPA** recurso no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo do Angular para uma marca fictícia de estilo de vida, a WKND. O aplicativo Angular é desenvolvido e projetado para ser implantado com o editor SPA do AEM, que mapeia componentes do Angular AEM para componentes do. O SPA completo, implantado no AEM, pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementação do WKND SPA*

## Sobre

O objetivo deste tutorial em várias partes é ensinar um desenvolvedor a implementar um aplicativo do Angular para funcionar com o recurso Editor de SPA do AEM. Em um cenário do mundo real, as atividades de desenvolvimento são divididas por persona, muitas vezes envolvendo um **Desenvolvedor de front-end** e uma **Desenvolvedor de back-end**. AEM Acreditamos que é benéfico que qualquer desenvolvedor envolvido em um projeto SPA Editor conclua este tutorial.

O tutorial foi projetado para funcionar com **AEM as a Cloud Service** e é compatível com versões anteriores com **AEM 6.5.4+** e **AEM 6.4.8+**. O SPA é implementado usando:

* [Arquétipo de projeto do AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR)
* [Editor SPA do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Angular](https://angular.io/)

*Estime de 1 a 2 horas para passar por cada parte do tutorial.*

## Código mais recente

Todo o código do tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A variável [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) O está disponível como Pacotes AEM para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Conhecimento básico do HTML, CSS e JavaScript
* Familiaridade básica com o [Angular](https://angular.io/)
* [SDK AS A CLOUD SERVICE AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) ou [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) e [npm](https://www.npmjs.com/)

*Embora não seja obrigatório, é benéfico ter uma compreensão [desenvolvimento de componentes tradicionais do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR).*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são capturados usando o SDK as a Cloud Service do AEM em execução em um ambiente de sistema operacional Mac com [Código do Visual Studio](https://code.visualstudio.com/) como o IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).
>
> **Novo no AEM 6.5?** Confira o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o [Projeto do Editor de SPA](create-project.md) e saiba como gerar um projeto habilitado para o Editor de SPA usando o Arquétipo de projeto AEM.

## Compatibilidade com versões anteriores {#compatibility}

O código de projeto deste tutorial foi criado para o AEM as a Cloud Service. Para tornar o código do projeto compatível com versões anteriores para **6.5.4+** e **6.4.8+** várias modificações foram feitas.

A variável [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** foi incluído como uma dependência:

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

Um perfil Maven adicional, chamado `classic` foi adicionado para modificar a criação para ambientes AEM 6.x do target:

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

A variável `classic` O perfil do está desativado por padrão. Se estiver seguindo o tutorial com AEM 6.x, adicione a variável `classic` perfil sempre que instruído a executar uma compilação Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao gerar um novo projeto para uma implementação do AEM, sempre use a versão mais recente do [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) e atualize o `aemVersion` para direcionar a versão pretendida do AEM.
