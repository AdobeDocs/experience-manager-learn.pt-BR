---
title: 'Introdução ao editor de SPA e SPA remoto: visão geral'
description: Boas vindas ao tutorial em várias partes para desenvolvedores que desejam ampliar um SPA remoto existente com conteúdo editável do AEM por meio do editor de SPA do AEM.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 100%

---

# Visão geral

{{edge-delivery-services}}

Boas vindas ao tutorial em várias partes para desenvolvedores que desejam ampliar um SPA remoto existente baseado em React (ou Next.js) com conteúdo editável do AEM por meio do editor de SPA do AEM.

Este tutorial baseia-se no [aplicativo em GraphQL da WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=pt-BR), um aplicativo em React que consome conteúdo de fragmentos de conteúdo do AEM nas APIs em GraphQL do AEM. No entanto, ele não fornece nenhuma criação com contexto de conteúdo de SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Sobre o tutorial

O tutorial tem como objetivo ilustrar como um SPA remoto ou um SPA executado fora do contexto do AEM pode ser atualizado para consumir e fornecer conteúdo criado no AEM.

A maioria das atividades do tutorial se concentra no desenvolvimento de JavaScript, mas também são abordados aspectos cruciais associados ao AEM. Esses aspectos incluem definir onde o conteúdo é criado e armazenado no AEM e como mapear rotas do SPA para páginas do AEM.

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e consiste em dois projetos:

1. O __projeto do AEM__ contém a configuração e o conteúdo que devem ser implantados no AEM.
1. O projeto do __aplicativo da WKND__ é o SPA a ser integrado ao editor de SPA do AEM

## Código mais recente

+ O ponto de partida do código deste tutorial pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial), na pasta `remote-spa-tutorial`.

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=pt-BR)
+ [Node.js v18](https://nodejs.org/pt)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [Código-fonte de aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Este tutorial pressupõe que você possui:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Um diretório de trabalho de `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Execução do SDK do AEM como um serviço de criação no `http://localhost:4502`
+ Executar o SDK do AEM com a conta `admin` local e a senha `admin`
+ Execução do SPA em `http://localhost:3000`

>[!NOTE]
>
> **Precisa de ajuda para configurar o seu ambiente de desenvolvimento local?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).

## &#x200B;1. Configurar o AEM para o editor de SPA

As configurações do AEM são necessárias para integrar o SPA ao editor de SPA do AEM. Essas configurações são gerenciadas e implantadas por meio de um projeto do AEM. Neste capítulo, saiba quais configurações são necessárias e como defini-las.

+ [Saiba como configurar o AEM para o editor de SPA](./aem-configure.md)

## &#x200B;2. Inicializar o SPA

Para que o editor de SPA do AEM integre um SPA a seu contexto de criação, é necessário adicionar alguns elementos ao SPA.

+ [Saiba como inicializar o SPA para o editor de SPA do AEM](./spa-bootstrap.md)

## &#x200B;3. Componentes fixos editáveis

Primeiro, aprenda a adicionar um “componente fixo” editável ao SPA. Isso ilustra como um desenvolvedor pode inserir um componente editável específico no SPA. Embora o criador possa alterar o conteúdo do componente, não é possível remover o componente nem alterar sua inserção, posicionamento ou tamanho.

+ [Saiba mais sobre componentes fixos editáveis](./spa-fixed-component.md)

## &#x200B;4. Componentes de container editáveis

Em seguida, aprenda a adicionar um “componente de container” editável ao SPA. Isso ilustra como um(a) desenvolvedor(a) pode inserir um componente de container no SPA. Os componentes de container permitem que os criadores insiram o componente permitido e ajustem o layout dos componentes.

+ [Saiba mais sobre componentes de container editáveis](./spa-container-component.md)

## &#x200B;5. Rotas dinâmicas e componentes editáveis

Por fim, use os conceitos explicados nos capítulos anteriores sobre rotas dinâmicas, as quais exibem um conteúdo diferente com base no parâmetro da rota em questão. Isso ilustra como o editor de SPA do AEM pode ser usado para criar conteúdo em rotas orientadas e derivadas de forma programática.

+ [Saiba mais sobre rotas dinâmicas e componentes editáveis](./spa-dynamic-routes.md)

## Recursos adicionais

+ [Componentes editáveis do editor de SPA do AEM](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
