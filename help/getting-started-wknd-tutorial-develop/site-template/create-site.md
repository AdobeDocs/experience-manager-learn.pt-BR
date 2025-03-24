---
title: Criar um site | Criação rápida de site do AEM
description: Saiba como usar o assistente de Criação de sites para gerar um novo site. O modelo de site padrão fornecido pelo Adobe é um ponto de partida para o novo site.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7496
thumbnail: KT-7496.jpg
doc-type: Tutorial
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
duration: 198
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 0%

---

# Criar um site {#create-site}

Como parte da Criação rápida de sites, use o Assistente de criação de sites no Adobe Experience Manager, AEM, para gerar um novo site. O modelo de site padrão fornecido pelo Adobe é usado como ponto de partida para o novo site.

## Pré-requisitos {#prerequisites}

As etapas deste capítulo ocorrerão em um ambiente Adobe Experience Manager as a Cloud Service. Verifique se você tem acesso administrativo ao ambiente do AEM. É recomendável usar um [Programa de sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) e [Ambiente de desenvolvimento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) ao concluir este tutorial.

Os ambientes do [Programa de produção](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) também podem ser usados para este tutorial; no entanto, verifique se as atividades deste tutorial não afetarão o trabalho que está sendo executado nos ambientes de destino, pois este tutorial implanta conteúdo e código no ambiente de destino do AEM.

O [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) pode ser usado para partes deste tutorial. Alguns aspectos deste tutorial que dependem dos serviços em nuvem, como a [implantação de temas com o pipeline de front-end do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html), não podem ser executados no AEM SDK.

Revise a [documentação de integração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) para obter mais detalhes.

## Objetivo {#objective}

1. Saiba como usar o Assistente de criação de site para gerar um novo site.
1. Entenda a função dos Modelos de site.
1. Explore o site do AEM gerado.

## Faça logon no Adobe Experience Manager Author {#author}

Como primeira etapa, faça logon no ambiente do AEM as a Cloud Service. Os ambientes do AEM estão divididos entre um **Serviço do autor** e um **Serviço de publicação**.

* **Serviço de Autor** - onde o conteúdo do site é criado, gerenciado e atualizado. Normalmente, apenas usuários internos têm acesso ao **Serviço do Autor** e estão atrás de uma tela de logon.
* **Serviço de Publicação** - hospeda o site ativo. Esse é o serviço que os usuários finais verão e que normalmente está disponível publicamente.

A maioria dos tutoriais será realizada usando o **Serviço do autor**.

1. Navegue até Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Faça logon usando sua Conta pessoal ou uma Conta da empresa/escola.
1. Verifique se a Organização correta está selecionada no menu e clique em **Experience Manager**.

   ![Experience Cloud Home](assets/create-site/experience-cloud-home-screen.png)

1. Em **Cloud Manager**, clique em **Iniciar**.
1. Passe o mouse sobre o Programa que deseja usar e clique no ícone **Programa Cloud Manager**.

   ![Ícone do programa Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. No menu superior, clique em **Ambientes** para exibir os ambientes provisionados.

1. Encontre o ambiente que você deseja usar e clique no **URL do autor**.

   ![Autor de desenvolvimento do Access](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >É recomendável usar um ambiente de **Desenvolvimento** para este tutorial.

1. Uma nova guia é iniciada para o **Serviço de Autor** do AEM. Clique em **Entrar com o Adobe** e você deverá fazer logon automaticamente com as mesmas credenciais do Experience Cloud.

1. Após ser redirecionado e autenticado, você deve ver a tela inicial do AEM.

   ![Tela inicial do AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Problemas para acessar o Experience Manager? Revise a [documentação de integração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Baixar o modelo de site básico

Um Modelo de site fornece um ponto de partida para um novo site. Um Modelo de site inclui alguns temas básicos, modelos de página, configurações e conteúdo de amostra. Cabe ao desenvolvedor determinar exatamente o que está incluído no Modelo de site. A Adobe fornece um **Modelo básico de site** para acelerar novas implementações.

1. Abra uma nova guia do navegador e navegue até o projeto Modelo básico de site no GitHub: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). O projeto é de código aberto e licenciado para ser usado por qualquer pessoa.
1. Clique em **Versões** e navegue até a [última versão](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Expanda a lista suspensa **Assets** e baixe o arquivo zip do modelo:

   ![Zip de Modelo de Site Básico](assets/create-site/template-basic-zip-file.png)

   Esse arquivo zip é usado no próximo exercício.

   >[!NOTE]
   >
   > Este tutorial é escrito usando a versão **1.1.0** do Modelo de site básico. Ao iniciar um novo projeto para uso de produção, é sempre recomendável usar a versão mais recente.

## Criar um novo site

Em seguida, gere um novo site usando o Modelo de site do exercício anterior.

1. Retorne ao ambiente do AEM. Na tela inicial do AEM, navegue até **Sites**.
1. No canto superior direito, clique em **Criar** > **Site (Modelo)**. Isso exibirá o **Assistente de Criação de Site**.
1. Em **Selecione um modelo de site**, clique no botão **Importar**.

   Carregue o arquivo de modelo **.zip** baixado do exercício anterior.

1. Selecione o **Modelo Básico de Site do AEM** e clique em **Avançar**.

   ![Selecionar modelo de site](assets/create-site/select-site-template.png)

1. Em **Detalhes do Site** > **Título do site**, digite `WKND Site`.

   Em uma implementação real, o &quot;Site WKND&quot; seria substituído pelo nome da marca da sua empresa ou organização. Neste tutorial, estamos simulando a criação de um site para uma marca fictícia de estilo de vida &quot;WKND&quot;.

1. Em **Nome do site**, digite `wknd`.

   ![Detalhes do Modelo de Site](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Se estiver usando um ambiente AEM compartilhado, anexe um identificador exclusivo ao **Nome do Site**. Por exemplo `wknd-site-johndoe`. Isso garantirá que vários usuários possam concluir o mesmo tutorial, sem colisões.

1. Clique em **Criar** para gerar o Site. Clique em **Concluído** na caixa de diálogo **Êxito** quando a AEM terminar de criar o site.

## Explorar o novo site

1. Navegue até o console AEM Sites, se ainda não estiver lá.
1. Um novo **Site WKND** foi gerado. Ele incluirá uma estrutura de site com uma hierarquia multilíngue.
1. Abra a página **Inglês** > **Página inicial** selecionando a página e clicando no botão **Editar** na barra de menus:

   ![Hierarquia do Site WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. O conteúdo inicial já foi criado e vários componentes estão disponíveis para serem adicionados a uma página. Experimente esses componentes para ter uma ideia da funcionalidade. Você aprenderá as noções básicas de um componente no próximo capítulo.

   ![Iniciar página inicial](assets/create-site/start-home-page.png)

   *Conteúdo de exemplo fornecido pelo Modelo de Site*

## Parabéns. {#congratulations}

Parabéns, você acabou de criar seu primeiro site do AEM!

### Próximas etapas {#next-steps}

Use o Editor de páginas no Adobe Experience Manager, AEM, para atualizar o conteúdo do site no capítulo [Criar conteúdo e publicar](author-content-publish.md). Saiba como os Componentes atômicos podem ser configurados para atualizar conteúdo. Entenda a diferença entre um Autor do AEM e os ambientes de Publicação e saiba como publicar atualizações no site ativo.
