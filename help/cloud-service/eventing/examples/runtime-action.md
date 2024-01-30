---
title: Eventos de ação e AEM do Adobe I/O Runtime
description: Saiba como receber Eventos AEM usando a ação do Adobe I/O Runtime e revisar os detalhes do evento, como carga, cabeçalhos e metadados.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---


# Eventos de ação e AEM do Adobe I/O Runtime

Saiba como receber eventos de AEM usando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Ação e revisão dos detalhes do evento, como carga, cabeçalhos e metadados.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

O Adobe I/O Runtime é uma plataforma sem servidor que permite a execução de código em resposta a eventos Adobe I/O. Dessa forma, você pode criar aplicativos orientados por eventos sem se preocupar com a infraestrutura.

Neste exemplo, você cria uma Adobe I/O Runtime [Ação](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) que recebe Eventos AEM e registra os detalhes do evento.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

As etapas de alto nível são:

- Criar projeto no Console do Adobe Developer
- Inicializar projeto para desenvolvimento local
- Configurar projeto no console do Adobe Developer
- Acione o evento AEM e verifique a execução da ação

## Pré-requisitos

Para concluir este tutorial, você precisa:

- Ambiente as a Cloud Service AEM com [Evento AEM ativado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Acesso a [Console do Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [CLI do Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalado no computador local.

## Criar projeto no Console do Adobe Developer

Para criar um projeto no Console do Adobe Developer, siga estas etapas:

- Navegue até [Console do Adobe Developer](https://developer.adobe.com/) e clique em **Console** botão.

- No **Início rápido** clique em **Criar projeto a partir de modelo**. Em seguida, no **Procurar modelos** , selecione **Construtor de aplicativos** modelo.

- Atualize o título do projeto, o nome do aplicativo e adicione o espaço de trabalho, se necessário. Em seguida, clique em **Salvar**.

  ![Criar projeto no Console do Adobe Developer](../assets/examples/runtime-action/create-project.png)


## Inicializar projeto para desenvolvimento local

Para adicionar uma Ação do Adobe I/O Runtime ao projeto, você deve inicializar o projeto para desenvolvimento local. No terminal aberto do computador local, navegue até o local em que deseja inicializar o projeto e siga estas etapas:

- Inicializar projeto executando

  ```bash
  aio app init
  ```

- Selecione o `Organization`, o `Project` criado na etapa anterior, e o espaço de trabalho. Entrada `What templates do you want to search for?` etapa, selecione `All Templates` opção.

  ![Org-Project-Selection - Inicializar projeto](../assets/examples/runtime-action/all-templates.png)

- Na lista de modelos, selecione `@adobe/generator-app-excshell` opção.

  ![Modelo de extensibilidade - Inicializar projeto](../assets/examples/runtime-action/extensibility-template.png)

- Abra o projeto em seu IDE favorito, por exemplo, VSCode.

- O selecionado _Modelo de extensibilidade_ (`@adobe/generator-app-excshell`) fornece uma ação de tempo de execução genérica, o código está em `src/dx-excshell-1/actions/generic/index.js` arquivo. Vamos atualizá-la para manter a simplicidade, registrar os detalhes do evento e retornar uma resposta de sucesso. No entanto, no próximo exemplo, ele é aprimorado para processar os Eventos AEM recebidos.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- Por fim, implante a ação atualizada no Adobe I/O Runtime executando.

  ```bash
  aio app deploy
  ```

## Configurar projeto no console do Adobe Developer

Para receber Eventos AEM e executar a Ação do Adobe I/O Runtime criada na etapa anterior, configure o projeto no Adobe Developer Console.

- No Console do Adobe Developer, navegue até o [projeto](https://developer.adobe.com/console/projects) criado na etapa anterior e clique em para abri-lo. Selecione o `Stage` espaço de trabalho, é aqui que a ação é implantada.

- Clique em **Adicionar serviço** e selecione **API** opção. No **Adicionar uma API** modal, selecione **Serviços da Adobe** > **API de gerenciamento de E/S** e clique em **Próxima**, siga as etapas adicionais de configuração e clique em **Salvar API configurada**.

  ![Adicionar serviço - Configurar projeto](../assets/examples/runtime-action/add-io-management-api.png)

- Da mesma forma, clique em **Adicionar serviço** e selecione **Evento** opção. No **Adicionar eventos** , selecione **Experience Cloud** > **AEM Sites** e clique em **Próxima**. Siga as etapas adicionais de configuração, selecione a instância do AEM, os tipos de evento e outros detalhes.

- Por último, no **Como receber eventos** etapa, expandir **Ação em tempo de execução** e selecione a opção _genérico_ ação criada na etapa anterior. Clique em **Salvar eventos configurados**.

  ![Ação em tempo de execução - Configurar projeto ](../assets/examples/runtime-action/select-runtime-action.png)

- Revise os detalhes do Registro de evento, também a **Rastreamento de depuração** e verifique a **Sonda Challenge** solicitação e resposta.

  ![Detalhes de inscrição no evento](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Acionar eventos de AEM

Para acionar eventos de AEM do seu ambiente as a Cloud Service AEM que foi registrado no projeto do Adobe Developer Console acima, siga estas etapas:

- Acesse e faça logon no ambiente de autor do AEM as a Cloud Service por meio do [Cloud Manager](https://my.cloudmanager.adobe.com/).

- Dependendo do **Eventos Assinados**, criar, atualizar, excluir, publicar ou desfazer a publicação de um fragmento de conteúdo.

## Revisar detalhes do evento

Depois de concluir as etapas acima, você deve ver os Eventos de AEM sendo entregues à ação genérica.

Você pode revisar os detalhes do evento na **Rastreamento de depuração** dos detalhes de Registro de evento.

![Detalhes do evento AEM](../assets/examples/runtime-action/aem-event-details.png)


## Próximas etapas

No próximo exemplo, vamos aprimorar essa ação para processar eventos do AEM, chamar de volta o serviço de autor do AEM para obter detalhes do conteúdo, armazenar detalhes no armazenamento do Adobe I/O Runtime SPA e exibi-los por meio do aplicativo de página única ().

