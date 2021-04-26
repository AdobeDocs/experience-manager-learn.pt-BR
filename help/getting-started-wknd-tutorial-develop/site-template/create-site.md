---
title: Criar um site
seo-title: Introdução ao AEM Sites - Criar um site
description: Use o Assistente de Criação de Site no Adobe Experience Manager, AEM, para gerar um novo site. O modelo de site padrão fornecido pelo Adobe é usado como ponto de partida para o novo site.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gerenciamento de conteúdo
feature: Componentes principais, Editor de páginas
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---


# Criar um site {#create-site}

>[!CAUTION]
>
> Os recursos rápidos de criação de sites mostrados aqui serão lançados na segunda metade de 2021. A documentação relacionada está disponível para fins de visualização.

Este capítulo aborda a criação de um novo site no Adobe Experience Manager. O Modelo de site padrão, fornecido pelo Adobe, é usado como ponto de partida.

## Pré-requisitos {#prerequisites}

As etapas neste capítulo ocorrerão em um ambiente Adobe Experience Manager as a Cloud Service. Certifique-se de ter acesso administrativo ao ambiente de AEM. É recomendável usar um [programa Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) e [Ambiente de desenvolvimento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) ao concluir este tutorial.

Revise a [documentação de integração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) para obter mais detalhes.

## Objetivo {#objective}

1. Saiba como usar o Assistente de criação de site para gerar um novo site.
1. Entender a função dos Modelos de site.
1. Explore o site de AEM gerado.

## Faça logon no autor do Adobe Experience Manager {#author}

Como primeira etapa, faça logon no AEM como um ambiente de Cloud Service. AEM ambientes são divididos entre um **Serviço de Autor** e um **Serviço de Publicação**.

* **Serviço de criação**  - onde o conteúdo do site é criado, gerenciado e atualizado. Normalmente, somente os usuários internos têm acesso ao **Serviço de criação** e está atrás de uma tela de logon.
* **Serviço de publicação**  - hospeda o site ativo. Esse é o serviço que os usuários finais verão e normalmente está disponível publicamente.

A maioria do tutorial ocorrerá usando o **Serviço de autor**.

1. Navegue até o Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Faça logon usando sua conta pessoal ou uma conta da empresa/escola.
1. Verifique se a Organização correta está selecionada no menu e clique em **Experience Manager**.

   ![Experience Cloud Home](assets/create-site/experience-cloud-home-screen.png)

1. Em **Cloud Manager** clique em **Iniciar**.
1. Passe o mouse sobre o Programa que deseja usar e clique no ícone **Cloud Manager Program**.

   ![Ícone do programa Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. No menu superior, clique em **Ambientes** para visualizar os ambientes provisionados.

1. Encontre o ambiente que deseja usar e clique no **URL do autor**.

   ![Autor do desenvolvimento de acesso](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >É recomendável usar um ambiente de **Desenvolvimento** para este tutorial.

1. Uma nova guia será iniciada no AEM **Author Service**. Clique em **Fazer logon com Adobe** e você deve fazer logon automaticamente com as mesmas credenciais de Experience Cloud.

1. Depois de ser redirecionado e autenticado, você verá a tela inicial AEM.

   ![Tela inicial do AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Está tendo problemas para acessar o Experience Manager? Revise a [documentação de integração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Baixe o modelo básico do site

Um Modelo de site fornece um ponto de partida para um novo site. Um Modelo de site inclui alguns temas básicos, modelos de página, configurações e conteúdo de exemplo. Exatamente o que está incluído no Modelo de site depende do desenvolvedor. O Adobe fornece um **Modelo de site básico** para acelerar novas implementações.

1. Abra uma nova guia do navegador e navegue até o projeto de modelo de site básico no GitHub: [https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic). O projeto é de código aberto e licenciado para ser usado por qualquer pessoa.
1. Clique em **Versões** e navegue até [versão mais recente](https://github.com/adobe/aem-site-template-basic/releases/latest).
1. Expanda a lista suspensa **Assets** e baixe o arquivo zip do modelo:

   ![Zip de Modelo de Site Básico](assets/create-site/template-basic-zip-file.png)

   Esse arquivo zip será usado no próximo exercício.

   >[!NOTE]
   >
   > Este tutorial é gravado usando a versão **5.0.0** do Modelo de site básico. Ao iniciar um novo projeto, é sempre recomendável usar a versão mais recente.

## Criar um novo site

Em seguida, gere um novo site usando o Modelo de Site do exercício anterior.

1. Retorne ao ambiente AEM. Na tela inicial AEM, navegue até **Sites**.
1. No canto superior direito, clique em **Create** > **Site (Template)**. Isso exibirá o **Assistente para Criar Site**.
1. Em **Selecionar um modelo de site** clique no botão **Importar**.

   Faça upload do arquivo de modelo **.zip** baixado do exercício anterior.

1. Selecione o **Modelo de Site Básico AEM** e clique em **Próximo**.

   ![Selecionar modelo de site](assets/create-site/select-site-template.png)

1. Em **Detalhes do site** > **Título do site** digite `WKND Site`.
1. Em **Nome do site** digite `wknd`.

   ![Detalhes do modelo de site](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Se estiver usando um ambiente de AEM compartilhado, anexe um identificador exclusivo ao **Nome do site**. Por exemplo `wknd-johndoe`. Isso garantirá que vários usuários possam concluir o mesmo tutorial, sem conflitos.

1. Clique em **Criar** para gerar o Site. Clique em **Concluído** na caixa de diálogo **Sucesso** quando AEM terminar de criar o site.

## Explore o novo site

1. Navegue até o console AEM Sites , se ainda não estiver lá.
1. Um novo **Site WKND** foi gerado. Ele incluirá uma estrutura de site com uma hierarquia multilíngue.
1. Abra a página **Inglês** > **Início** selecionando a página e clicando no botão **Editar** na barra de menus:

   ![Hierarquia de Site WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. O conteúdo inicial já foi criado e vários componentes estão disponíveis para serem adicionados a uma página. Experimente esses componentes para ter uma ideia da funcionalidade. Você aprenderá as noções básicas de um componente no próximo capítulo.

   ![Iniciar página inicial](assets/create-site/start-home-page.png)

   *Amostra de conteúdo fornecida pelo modelo do site*

## Parabéns! {#congratulations}

Parabéns, você acabou de criar seu primeiro site AEM!

### Próximas etapas {#next-steps}

Use o Editor de páginas no Adobe Experience Manager, AEM, para atualizar o conteúdo do site no capítulo [Autor de conteúdo e publicar](author-content-publish.md). Saiba como os Componentes atômicos podem ser configurados para atualizar o conteúdo. Entenda a diferença entre um autor do AEM e ambientes de publicação e saiba como publicar atualizações no site ativo.
