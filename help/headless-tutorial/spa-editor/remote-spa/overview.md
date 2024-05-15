---
title: Introdução ao Editor de SPA e SPA remoto - Visão geral
description: Bem-vindo ao tutorial em várias partes para desenvolvedores que buscam aumentar um SPA remoto existente com conteúdo de AEM editável usando o editor de AEM SPA.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 6%

---

# Visão geral

{{edge-delivery-services}}

Bem-vindo ao tutorial em várias partes para desenvolvedores que buscam aumentar um SPA remoto baseado no React (ou Next.js) existente com conteúdo de AEM editável usando o Editor de SPA AEM.

Este tutorial se baseia no [Aplicativo WKND GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=pt-BR), um aplicativo do React que consome conteúdo de Fragmento de conteúdo do AEM por meio de APIs do AEM, no entanto, não fornece nenhuma criação em contexto de conteúdo do GraphQL SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Sobre o tutorial

O tutorial destinado a ilustrar como um SPA remoto, ou SPA executado fora do contexto do AEM AEM, pode ser atualizado para consumir e entregar conteúdo criado no.

A maioria das atividades no tutorial se concentra no desenvolvimento do JavaScript, no entanto, são abordados aspectos críticos que giram em torno do AEM. Esses aspectos incluem definir onde o conteúdo é criado e armazenado no AEM e mapear rotas do SPA para páginas do AEM.

O tutorial foi projetado para funcionar com **AEM as a Cloud Service** e é composto por dois projetos:

1. A variável __Projeto AEM__ contém a configuração e o conteúdo que devem ser implantados no AEM.
1. __Aplicativo WKND__ O projeto é o SPA para ser integrado ao SPA Editor do AEM

## Código mais recente

+ O ponto de partida do código deste tutorial pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) no `remote-spa-tutorial` pasta.

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql código-fonte](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Este tutorial pressupõe:

+ [Código do Microsoft® Visual Studio](https://visualstudio.microsoft.com/) como o IDE
+ Um diretório de trabalho de `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Execução do SDK do AEM como um serviço do autor no `http://localhost:4502`
+ Execução do SDK do AEM com o local `admin` conta com senha `admin`
+ Executando o SPA `http://localhost:3000`

>[!NOTE]
>
> **Precisa de ajuda para configurar o ambiente de desenvolvimento local?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).

## 1. Configurar o SPA para o Editor de AEM

Configurações de AEM são necessárias para integrar o SPA com o editor de AEM SPA. Essas configurações são gerenciadas e implantadas por meio de um projeto AEM. Neste capítulo, saiba quais configurações são necessárias e como defini-las.

+ [Saiba como configurar o AEM para o Editor SPA](./aem-configure.md)

## 2. Bootstrap do SPA

Para o SPA Editor integrar um SPA SPA ao contexto de criação, algumas adições devem ser feitas ao AEM.

+ [Saiba como inicializar o SPA AEM para SPA Editor](./spa-bootstrap.md)

## 3. Componentes fixos editáveis

Primeiro, explore a adição de um &quot;componente fixo&quot; editável ao SPA. Isso ilustra como um desenvolvedor pode colocar um componente editável específico no SPA. Embora o autor possa alterar o conteúdo do componente, ele não pode remover o componente ou alterar sua disposição, posicionamento ou tamanho.

+ [Saiba mais sobre componentes fixos editáveis](./spa-fixed-component.md)

## 4. Componentes editáveis do contêiner

Em seguida, explore a adição de um &quot;componente de contêiner&quot; editável ao SPA. Isso ilustra como um desenvolvedor pode colocar um componente de contêiner no SPA. Os componentes do contêiner permitem que os autores coloquem o componente permitido nele e ajustem o layout dos componentes.

+ [Saiba mais sobre componentes editáveis do contêiner](./spa-container-component.md)

## 5. Rotas dinâmicas e componentes editáveis

Por fim, use os conceitos explicados nos capítulos anteriores para rotas dinâmicas; rotas que exibem conteúdo diferente com base no parâmetro da rota. AEM Isso ilustra como o Editor de SPA pode ser usado para criar conteúdo em rotas que são orientadas e derivadas de forma programática.

+ [Saiba mais sobre rotas dinâmicas e componentes editáveis](./spa-dynamic-routes.md)

## Recursos adicionais

+ [AEM Componentes editáveis do SPA React](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
