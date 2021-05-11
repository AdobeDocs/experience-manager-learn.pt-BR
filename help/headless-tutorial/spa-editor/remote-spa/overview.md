---
title: Introdução ao Editor SPA e SPA remotos - Visão geral
description: Bem-vindo ao tutorial de várias partes para desenvolvedores que procuram aumentar um SPA Remoto existente com conteúdo AEM editável usando AEM Editor SPA.
topic: Sem periféricos, SPA, desenvolvimento
feature: Editor de SPA, Componentes principais, APIs, Desenvolvimento
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: kt-7630.jpg
translation-type: tm+mt
source-git-commit: ec692af56afa83330097bb9d8db0d2f2f4fde1c1
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 6%

---


# Visão geral

Bem-vindo ao tutorial de várias partes para desenvolvedores que procuram aumentar um SPA Remoto baseado em Reação (ou Next.js) existente com conteúdo AEM editável usando AEM Editor SPA.

Este tutorial baseia-se no [Aplicativo GraphQL WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html), um aplicativo React que consome AEM conteúdo do Fragmento de conteúdo sobre AEM APIs GraphQL, no entanto, não fornece nenhuma criação de contexto de conteúdo SPA.

## Sobre o tutorial

O tutorial tem como objetivo ilustrar como um SPA Remoto, ou um SPA executado fora do contexto de AEM, pode ser atualizado para consumir e fornecer conteúdo criado em AEM.

A maioria das atividades no tutorial se concentra no desenvolvimento do JavaScript, no entanto, os aspectos críticos são abordados e giram em torno do AEM. Esses aspectos incluem a definição de onde o conteúdo é criado e armazenado em AEM e o mapeamento SPA rotas para páginas AEM.

O tutorial foi projetado para funcionar com **AEM como um Cloud Service** e é composto de dois projetos:

1. O __AEM Projeto__ contém configuração e conteúdo que devem ser implantados em AEM.
1. __O__ Appproject WKND é o SPA a ser integrado AEM Editor SPA

## Código mais recente

+ O código deste tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) na ramificação `feature/spa-editor`.

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql source code](https://github.com/adobe/aem-guides-wknd-graphql)

Este tutorial presume:

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas o IDE
+ Um diretório de trabalho de `~/Code/wknd-app`
+ Executar o SDK do AEM como um serviço de Autor em `http://localhost:4502`
+ Execução do SDK AEM com a conta local `admin` com a senha `admin`
+ Execução do SPA em `http://localhost:3000`

>[!NOTE]
>
> **Precisa de ajuda para configurar seu ambiente de desenvolvimento local?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).


## Configuração rápida

A Configuração rápida ativa você com o SPA de aplicativos WKND e AEM Editor de SPA em 15 minutos. Essa configuração acelerada leva você diretamente ao estado final do tutorial, permitindo explorar a criação do SPA no AEM Editor SPA.

+ [Saiba mais sobre a configuração rápida](./quick-setup.md)

## 1. Configurar AEM para SPA Editor

AEM configurações são necessárias para integrar o SPA com AEM Editor de SPA. Essas configurações são gerenciadas e implantadas por meio de um AEM Project. Neste capítulo, saiba mais sobre as configurações necessárias e como defini-las.

+ [Saiba como configurar o AEM para SPA Editor](./aem-configure.md)

## 2. Bootstrap do SPA

Para que AEM SPA Editor integre um SPA ao seu contexto de criação, algumas adições devem ser feitas ao SPA.

+ [Saiba como inicializar o SPA para AEM Editor SPA](./spa-bootstrap.md)

## 3. Componentes fixos editáveis

Primeiro, explore a adição de um &quot;componente fixo&quot; editável ao SPA. Isso ilustra como um desenvolvedor pode colocar um componente editável específico no SPA. Embora o autor possa alterar o conteúdo do componente, ele não pode remover o componente ou alterar seu posicionamento, posicionamento ou tamanho.

+ [Saiba mais sobre componentes fixos editáveis](./spa-fixed-component.md)

## 4. Componentes editáveis do contêiner

Em seguida, explore a adição de um &quot;componente de contêiner&quot; editável ao SPA. Isso ilustra como um desenvolvedor pode colocar um componente de contêiner no SPA. Os componentes do contêiner permitem que os autores coloquem o componente permitido nele e ajustem o layout dos componentes.

+ [Saiba mais sobre componentes de contêiner editáveis](./spa-container-component.md)

## 5. Rotas dinâmicas e componentes editáveis

Por último, utilizar os conceitos explicados nos capítulos anteriores para rotas dinâmicas; rotas que exibem conteúdo diferente com base no parâmetro da rota. Isso ilustra como AEM Editor de SPA pode ser usado para criar conteúdo em rotas que são orientadas e derivadas de forma programática.

+ [Saiba mais sobre rotas dinâmicas e componentes editáveis](./spa-dynamic-routes.md)

## Recursos adicionais

+ [Edição de um SPA externo no AEM Docs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [Componentes AEM do WCM - Implementação principal do React](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [Componentes AEM WCM - Editor Spa - Implementação principal do React](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
