---
title: Introdução ao Angular e o Editor de SPA do AEM
description: Criar o seu primeiro aplicativo de página única (SPA) do Angular que seja editável no Adobe Experience Manager (AEM), com SPA da WKND.
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
workflow-type: ht
source-wordcount: '583'
ht-degree: 100%

---

# Criar o seu primeiro SPA do Angular no AEM {#introduction}

{{edge-delivery-services}}

Boas-vindas a esse tutorial de várias partes, projetado para iniciantes no desenvolvimento com o recurso do **Editor de SPA** do Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um aplicativo do Angular para uma marca fictícia de estilo de vida, a WKND. O aplicativo do Angular foi desenvolvido e projetado para ser implantado com o editor de SPA do AEM, que mapeia componentes do Angular para componentes do AEM. O SPA concluído e implantado no AEM pode ser criado dinamicamente com as ferramentas tradicionais de edição em linha do AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementação do SPA da WKND*

## Sobre

O objetivo deste tutorial em várias partes é ensinar um desenvolvedor a implementar um aplicativo do Angular para funcionar com o recurso de editor de SPA do AEM. Em um caso real, as atividades de desenvolvimento são divididas por persona, geralmente envolvendo um **desenvolvedor de front-end** e um **desenvolvedor de back-end**. Acreditamos que fazer este tutorial seja benéfico para qualquer desenvolvedor envolvido em um projeto do editor de SPA do AEM.

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e conta com compatibilidade retroativa com o **AEM 6.5.4+** e o **AEM 6.4.8+**. O SPA é implementado por meio de:

* [Arquétipo de projeto do AEM Maven](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/developing/archetype/overview)
* [Editor de SPA do AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/developing/spas/spa-walkthrough#content-editing-experience-with-spa)
* [Componentes principais](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction)
* [Angular](https://angular.io/)

*Estimativa de 1 a 2 horas para passar por cada parte do tutorial.*

## Código mais recente

Todo o código do tutorial pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

A [base de código mais recente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponível como pacotes do AEM para download.

## Pré-requisitos

Antes de iniciar este tutorial, você precisará do seguinte:

* Conhecimento básico de HTML, CSS e JavaScript
* Familiaridade básica com o [Angular](https://angular.io/)
* [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/br/experience-manager/aem-releases-updates.html#65) ou [AEM 6.4.8+](https://helpx.adobe.com/br/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/pt) e [npm](https://www.npmjs.com/)

*Embora não seja obrigatório, é vantajoso ter uma compreensão básica do [desenvolvimento de componentes tradicionais do AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR).*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são captados por meio do SDK do AEM as a Cloud Service em execução em um ambiente macOS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## Próximas etapas {#next-steps}

O que você está esperando? Para começar o tutorial, navegue até o capítulo [Projeto do editor de SPA](create-project.md) e saiba como gerar um projeto habilitado para o editor de SPA por meio do arquétipo de projeto do AEM.

## Compatibilidade retroativa {#compatibility}

O código do projeto deste tutorial foi criado para o AEM as a Cloud Service. Para tornar o código do projeto compatível com as versões anteriores **6.5.4+** e **6.4.8+**, várias modificações foram feitas.

O [UberJar](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/developing/devtools/ht-projects-maven#what-is-the-uberjar) **v6.4.4** foi incluído como uma dependência:

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

Um perfil do Maven adicional, denominado `classic`, foi adicionado para modificar a compilação para ser usada em ambientes do AEM 6.x:

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

O perfil do `classic` é desabilitado por padrão. Se estiver seguindo o tutorial com o AEM 6.x, adicione o perfil do `classic` sempre que for necessário executar uma compilação do Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao gerar um novo projeto para uma implementação do AEM, sempre use a versão mais recente do [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) e atualize o `aemVersion` de acordo com a versão pretendida do AEM.
