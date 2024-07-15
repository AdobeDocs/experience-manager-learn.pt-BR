---
title: Processamento de eventos AEM usando a ação do Adobe I/O Runtime
description: Saiba como processar eventos AEM recebidos usando a ação do Adobe I/O Runtime.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---

# Processamento de eventos AEM usando a ação do Adobe I/O Runtime

Saiba como processar eventos AEM recebidos usando a Ação [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/). Este exemplo aprimora o exemplo anterior [Ação do Adobe I/O Runtime e Eventos AEM](runtime-action.md). Verifique se você o concluiu antes de continuar com este.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

Neste exemplo, o processamento de eventos armazena os dados originais do evento e o evento recebido como uma mensagem de atividade no armazenamento do Adobe I/O Runtime. No entanto, se o evento for do tipo _Fragmento de conteúdo modificado_, ele também chamará o serviço de autor do AEM para encontrar os detalhes de modificação. Por fim, exibe os detalhes do evento em um aplicativo de página única (SPA).

## Pré-requisitos

Para concluir este tutorial, você precisa:

- Ambiente AEM as a Cloud Service com [evento AEM habilitado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Além disso, o projeto [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) de amostra deve ser implantado nele.

- Acesso ao [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalada no computador local.

- Projeto inicializado localmente a partir do exemplo anterior [Ação do Adobe I/O Runtime e Eventos AEM](./runtime-action.md#initialize-project-for-local-development).

>[!IMPORTANT]
>
>O AEM as a Cloud Service Eventing só está disponível para usuários registrados no modo de pré-lançamento. Para habilitar eventos de AEM no seu ambiente AEM as a Cloud Service, entre em contato com a [equipe de eventos de AEM](mailto:grp-aem-events@adobe.com).

## Ação do processador de eventos AEM

Neste exemplo, a [ação](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) do processador de eventos executa as seguintes tarefas:

- Analisa o evento recebido em uma mensagem de atividade.
- Se o evento recebido for do tipo _Fragmento de Conteúdo Modificado_, chame de volta para o serviço de autor do AEM para encontrar os detalhes da modificação.
- Mantém os dados originais do evento, a mensagem de atividade e os detalhes de modificação (se houver) no armazenamento da Adobe I/O Runtime.

Para executar as tarefas acima, vamos começar adicionando uma ação ao projeto, desenvolver módulos do JavaScript para executar as tarefas acima e, finalmente, atualizar o código de ação para usar os módulos desenvolvidos.

Consulte o arquivo [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) anexado para obter o código completo, e a seção abaixo destaca os arquivos principais.

### Adicionar ação

- Para adicionar uma ação, execute o seguinte comando:

  ```bash
  aio app add action
  ```

- Selecione `@adobe/generator-add-action-generic` como modelo de ação, nomeie a ação como `aem-event-processor`.

  ![Adicionar ação](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Desenvolver módulos do JavaScript

Para executar as tarefas mencionadas acima, vamos desenvolver os seguintes módulos do JavaScript.

- O módulo `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` determina se o evento recebido é do tipo _Fragmento de conteúdo modificado_.

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- O módulo `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` chama o serviço de autor do AEM para localizar os detalhes da modificação.

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  Consulte o [tutorial sobre credenciais de serviço do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) para saber mais. Além disso, os [Arquivos de Configuração do App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) para gerenciar segredos e parâmetros de ação.

- O módulo `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` armazena os dados originais do evento, a mensagem de atividade e os detalhes de modificação (se houver) no armazenamento do Adobe I/O Runtime.

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### Atualizar código de ação

Finalmente, atualize o código de ação em `src/dx-excshell-1/actions/aem-event-processor/index.js` para usar os módulos desenvolvidos.

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## Recursos adicionais

- A pasta `src/dx-excshell-1/actions/model` contém arquivos `aemEvent.js` e `errors.js`, que são usados pela ação para analisar o evento recebido e manipular erros, respectivamente.
- A pasta `src/dx-excshell-1/actions/load-processed-aem-events` contém o código de ação. Essa ação é usada pelo SPA para carregar os Eventos AEM processados do armazenamento Adobe I/O Runtime.
- A pasta `src/dx-excshell-1/web-src` contém o código SPA, que exibe os Eventos AEM processados.
- O arquivo `src/dx-excshell-1/ext.config.yaml` contém parâmetros e configurações de ação.

## Conceito e principais pontos

Os requisitos de processamento de eventos diferem de projeto para projeto. No entanto, os principais argumentos deste exemplo são:

- O processamento do evento pode ser feito usando a Ação do Adobe I/O Runtime.
- A Ação em tempo de execução pode se comunicar com sistemas como seus aplicativos internos, soluções de terceiros e soluções de Adobe.
- A ação de tempo de execução serve como ponto de entrada para um processo de negócios projetado em torno de uma alteração de conteúdo.
