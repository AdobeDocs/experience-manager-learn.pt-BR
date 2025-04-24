---
title: Ação do Adobe I/O Runtime e eventos do AEM
description: Saiba como receber Eventos do AEM usando a ação do Adobe I/O Runtime e revisar os detalhes do evento, como carga, cabeçalhos e metadados.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# Ação do Adobe I/O Runtime e eventos do AEM

Saiba como receber Eventos do AEM usando a Ação [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) e revise os detalhes do evento, como carga, cabeçalhos e metadados.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

O Adobe I/O Runtime é uma plataforma sem servidor que permite a execução de código em resposta ao Adobe I/O Events. Dessa forma, você pode criar aplicativos orientados por eventos sem se preocupar com a infraestrutura.

Neste exemplo, você cria uma [Ação](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) do Adobe I/O Runtime que recebe Eventos do AEM e registra os detalhes do evento.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

As etapas de alto nível são:

- Criar projeto no Adobe Developer Console
- Inicializar projeto para desenvolvimento local
- Configurar projeto no Adobe Developer Console
- Acione o evento do AEM e verifique a execução da ação

## Pré-requisitos

Para concluir este tutorial, você precisa:

- Ambiente AEM as a Cloud Service com [Evento AEM habilitado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Acesso ao [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started).

- [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalada no computador local.

## Criar projeto no Adobe Developer Console

Para criar um projeto no Adobe Developer Console, siga estas etapas:

- Navegue até [Adobe Developer Console](https://developer.adobe.com/) e clique no botão **Console**.

- Na seção **Início Rápido**, clique em **Criar projeto a partir do modelo**. Em seguida, na caixa de diálogo **Procurar modelos**, selecione o modelo **App Builder**.

- Atualize o título do projeto, o nome do aplicativo e adicione o espaço de trabalho, se necessário. Em seguida, clique em **Salvar**.

  ![Criar projeto no Adobe Developer Console](../assets/examples/runtime-action/create-project.png)


## Inicializar projeto para desenvolvimento local

Para adicionar uma Ação do Adobe I/O Runtime ao projeto, você deve inicializar o projeto para desenvolvimento local. No terminal aberto do computador local, navegue até o local em que deseja inicializar o projeto e siga estas etapas:

- Inicializar projeto executando

  ```bash
  aio app init
  ```

- Selecione o `Organization`, o `Project` que você criou na etapa anterior e o espaço de trabalho. Na etapa `What templates do you want to search for?`, selecione a opção `All Templates`.

  ![Org-Project-Selection - Inicializar projeto](../assets/examples/runtime-action/all-templates.png)

- Na lista de modelos, selecione a opção `@adobe/generator-app-excshell`.

  ![Modelo de extensibilidade - Inicializar projeto](../assets/examples/runtime-action/extensibility-template.png)

- Abra o projeto em seu IDE favorito, por exemplo, VSCode.

- O _Modelo de extensibilidade_ (`@adobe/generator-app-excshell`) selecionado fornece uma ação de tempo de execução genérica; o código está no arquivo `src/dx-excshell-1/actions/generic/index.js`. Vamos atualizá-la para manter a simplicidade, registrar os detalhes do evento e retornar uma resposta de sucesso. No entanto, no próximo exemplo, ele é aprimorado para processar os Eventos AEM recebidos.

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

## Configurar projeto no Adobe Developer Console

Para receber Eventos do AEM e executar a Ação do Adobe I/O Runtime criada na etapa anterior, configure o projeto no Adobe Developer Console.

- No Adobe Developer Console, navegue até o [projeto](https://developer.adobe.com/console/projects) criado na etapa anterior e clique para abri-lo. Selecione o espaço de trabalho `Stage`. Aqui é onde a ação foi implantada.

- Clique no botão **Adicionar Serviço** e selecione a opção **API**. No modal **Adicionar uma API**, selecione **Adobe Services** > **API de Gerenciamento de E/S** e clique em **Avançar**, siga as etapas de configuração adicionais e clique em **Salvar API configurada**.

  ![Adicionar Serviço - Configurar projeto](../assets/examples/runtime-action/add-io-management-api.png)

- Da mesma forma, clique no botão **Adicionar Serviço** e selecione a opção **Evento**. Na caixa de diálogo **Adicionar Eventos**, selecione **Experience Cloud** > **AEM Sites** e clique em **Avançar**. Siga as etapas adicionais de configuração, selecione a instância do AEM, os tipos de evento e outros detalhes.

- Finalmente, na etapa **Como receber eventos**, expanda a opção **Ação em tempo de execução** e selecione a ação _genérica_ criada na etapa anterior. Clique em **Salvar eventos configurados**.

  ![Ação em tempo de execução - Configurar projeto ](../assets/examples/runtime-action/select-runtime-action.png)

- Revise os detalhes do Registro de eventos, também a guia **Rastreamento de depuração** e verifique a solicitação e a resposta do **Teste de desafio**.

  ![Detalhes de Inscrição no Evento](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Acionar eventos do AEM

Para acionar eventos do AEM a partir do ambiente do AEM as a Cloud Service que foi registrado no projeto do Adobe Developer Console acima, siga estas etapas:

- Acesse e faça logon no ambiente de criação do AEM as a Cloud Service via [Cloud Manager](https://my.cloudmanager.adobe.com/).

- Dependendo dos seus **Eventos nos quais você se inscreveu**, crie, atualize, exclua, publique ou cancele a publicação de um Fragmento de conteúdo.

## Revisar detalhes do evento

Depois de concluir as etapas acima, você deve ver os Eventos da AEM sendo entregues à ação genérica.

Você pode examinar os detalhes do evento na guia **Rastreamento de Depuração** dos detalhes de Registro de Eventos.

![Detalhes do Evento AEM](../assets/examples/runtime-action/aem-event-details.png)


## Próximas etapas

No próximo exemplo, vamos aprimorar essa ação para processar eventos do AEM, chamar o serviço de autor do AEM para obter detalhes do conteúdo, armazenar detalhes no armazenamento do Adobe I/O Runtime e exibi-los por meio de Aplicativo de página única (SPA).
