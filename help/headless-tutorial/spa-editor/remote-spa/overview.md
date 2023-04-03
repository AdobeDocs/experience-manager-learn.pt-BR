---
title: Introdução ao Editor SPA e SPA remotos - Visão geral
description: Bem-vindo ao tutorial de várias partes para desenvolvedores que procuram aumentar um SPA Remoto existente com conteúdo AEM editável usando AEM Editor SPA.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 10%

---

# Visão geral

Bem-vindo ao tutorial de várias partes para desenvolvedores que procuram aumentar um SPA Remoto baseado em Reação (ou Next.js) existente com conteúdo AEM editável usando AEM Editor SPA.

Este tutorial se baseia no [Aplicativo WKND GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=pt-BR), um aplicativo React que consome AEM conteúdo do Fragmento de conteúdo em vez AEM APIs do GraphQL, no entanto não fornece criação de conteúdo SPA contexto.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Sobre o tutorial

O tutorial tem como objetivo ilustrar como um SPA Remoto, ou um SPA executado fora do contexto de AEM, pode ser atualizado para consumir e fornecer conteúdo criado em AEM.

A maioria das atividades no tutorial se concentra no desenvolvimento do JavaScript, no entanto, os aspectos críticos são abordados e giram em torno do AEM. Esses aspectos incluem a definição de onde o conteúdo é criado e armazenado em AEM e o mapeamento SPA rotas para páginas AEM.

O tutorial foi projetado para funcionar com **AEM as a Cloud Service** e é composto por dois projetos:

1. O __Projeto AEM__ contém configuração e conteúdo que devem ser implantados em AEM.
1. __Aplicativo WKND__ projeto é o SPA a ser integrado ao AEM SPA Editor

## Código mais recente

+ O ponto de partida do código deste tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa) no `remote-spa-tutorial` pasta.

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql source code](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Este tutorial presume:

+ [Código Microsoft® Visual Studio](https://visualstudio.microsoft.com/) como o IDE
+ Um diretório de trabalho de `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Execução do SDK do AEM como um serviço de autor em `http://localhost:4502`
+ Execução do SDK do AEM com o `admin` conta com senha `admin`
+ Execução do SPA em `http://localhost:3000`

>[!NOTE]
>
> **Precisa de ajuda para configurar seu ambiente de desenvolvimento local?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).

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

+ [AEM SPA Reagir componentes editáveis](https://www.npmjs.com/package/@adobe/aem-react-editable-components)