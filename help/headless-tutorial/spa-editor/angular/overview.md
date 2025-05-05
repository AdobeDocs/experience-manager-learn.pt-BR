---
title: Introdução ao Editor e Angular de SPA do AEM
description: Criar o seu primeiro aplicativo de página única (SPA) Angular que seja editável no Adobe Experience Manager (AEM), com o WKND SPA.
version: Experience Manager as a Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 14%

---

# Criar seu primeiro SPA do Angular no AEM {#introduction}

{{edge-delivery-services}}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novos no **recurso Editor de SPA** do Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo do Angular para uma marca fictícia de estilo de vida, a WKND. O aplicativo Angular é desenvolvido e projetado para ser implantado com o editor de SPA da AEM, que mapeia componentes do Angular para componentes do AEM. O SPA concluído, implantado no AEM, pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA Final Implementado](assets/wknd-spa-implementation.png)

*Implementação WKND SPA*

## Sobre

O objetivo deste tutorial em várias partes é ensinar um desenvolvedor a implementar um aplicativo do Angular para funcionar com o recurso Editor de SPA do AEM. Em um cenário real, as atividades de desenvolvimento são divididas por persona, geralmente envolvendo um **desenvolvedor de front-end** e um **desenvolvedor de back-end**. Acreditamos que é benéfico para qualquer desenvolvedor envolvido em um projeto do Editor SPA do AEM concluir este tutorial.

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e tem compatibilidade retroativa com o **AEM 6.5.4+** e o **AEM 6.4.8+**. A SPA é implementada usando:

* [Arquétipo de projeto do AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR)
* [Editor SPA do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=pt-BR#content-editing-experience-with-spa)
* [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Angular](https://angular.io/)

*Estimativa de 1 a 2 horas para passar por cada parte do tutorial.*

## Código mais recente

Todo o código do tutorial pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponível como Pacotes do AEM para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Um conhecimento básico da HTML, CSS e JavaScript
* Familiaridade básica com o [Angular](https://angular.io/)
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=pt-BR#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/br/experience-manager/aem-releases-updates.html#65) ou [AEM 6.4.8+](https://helpx.adobe.com/br/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) e [npm](https://www.npmjs.com/)

*Embora não seja obrigatório, é benéfico ter uma compreensão básica do [desenvolvimento de componentes tradicionais do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR).*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são capturados usando o AEM as a Cloud Service SDK em execução em um ambiente Mac OS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).
>
> **Novo no AEM 6.5?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o capítulo [Projeto do editor de SPA](create-project.md) e saiba como gerar um projeto habilitado para o editor de SPA usando o Arquétipo de projeto do AEM.

## Compatibilidade com versões anteriores {#compatibility}

O código de projeto deste tutorial foi criado para o AEM as a Cloud Service. Para tornar o código de projeto compatível com versões anteriores para **6.5.4+** e **6.4.8+**, várias modificações foram feitas.

O [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=pt-BR#what-is-the-uberjar) **v6.4.4** foi incluído como uma dependência:

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

Um perfil Maven adicional, chamado `classic`, foi adicionado para modificar a compilação para ambientes AEM 6.x de destino:

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

O perfil `classic` está desabilitado por padrão. Se estiver seguindo o tutorial com o AEM 6.x, adicione o perfil `classic` sempre que for instruído a executar uma compilação Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao gerar um novo projeto para uma implementação do AEM, sempre use a versão mais recente do [Arquétipo de Projetos AEM](https://github.com/adobe/aem-project-archetype) e atualize o `aemVersion` para direcionar a versão pretendida do AEM.
