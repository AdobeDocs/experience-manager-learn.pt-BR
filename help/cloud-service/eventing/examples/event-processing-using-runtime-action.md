---
title: Processamento de eventos AEM usando a ação do Adobe I/O Runtime
description: Saiba como processar eventos AEM recebidos usando a ação do Adobe I/O Runtime.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
source-git-commit: f0930e517254b6353fe50c3bbf9ae915d9ef6ca3
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# Processamento de eventos AEM usando a ação do Adobe I/O Runtime

Saiba como processar eventos AEM recebidos usando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Ação. Este exemplo aprimora o anterior [Eventos de ação e AEM do Adobe I/O Runtime](runtime-action.md), verifique se você o concluiu antes de continuar com este.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

Neste exemplo, o processamento de eventos armazena os dados originais do evento e o evento recebido como uma mensagem de atividade no armazenamento do Adobe I/O Runtime. No entanto, se o evento for _Fragmento de conteúdo modificado_ , ele também chama o serviço de autor do AEM para encontrar os detalhes da modificação. Por fim, exibe os detalhes do evento em um aplicativo de página única (SPA).

## Pré-requisitos

Para concluir este tutorial, você precisa:

- Ambiente as a Cloud Service AEM com [Evento AEM ativado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Além disso, a amostra [Sites da WKND](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) O projeto deve ser implantado nele.

- Acesso a [Console do Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [CLI do Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalado no computador local.

- Projeto inicializado localmente a partir do exemplo anterior [Eventos de ação e AEM do Adobe I/O Runtime](./runtime-action.md#initialize-project-for-local-development).

>[!IMPORTANT]
>
>O evento as a Cloud Service de AEM só está disponível para usuários registrados no modo de pré-lançamento. Para habilitar o evento de AEM em seu ambiente as a Cloud Service AEM, entre em contato com [Equipe de evento do AEM](mailto:grp-aem-events@adobe.com).

## Ação do processador de eventos AEM

Neste exemplo, o processador de eventos [ação](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) O executa as seguintes tarefas:

- Analisa o evento recebido em uma mensagem de atividade.
- Se o evento recebido for de _Fragmento de conteúdo modificado_ digite, retorne a chamada ao serviço de autoria do AEM para encontrar os detalhes da modificação.
- Mantém os dados originais do evento, a mensagem de atividade e os detalhes de modificação (se houver) no armazenamento da Adobe I/O Runtime.

Para executar as tarefas acima, vamos começar adicionando uma ação ao projeto, desenvolver módulos JavaScript para executar as tarefas acima e, finalmente, atualizar o código de ação para usar os módulos desenvolvidos.

Consulte a guia anexada [WKND-AEM-Eventing-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) para obter o código completo, e abaixo da seção realça os arquivos principais.

### Adicionar ação

- Para adicionar uma ação, execute o seguinte comando:

  ```bash
  aio app add action
  ```

- Selecionar `@adobe/generator-add-action-generic` como modelo de ação, nomeie a ação como `aem-event-processor`.

  ![Adicionar ação](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### Desenvolver módulos JavaScript

Para executar as tarefas mencionadas acima, vamos desenvolver os seguintes módulos JavaScript.

- A variável `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` O módulo determina se o evento recebido é de _Fragmento de conteúdo modificado_ tipo.

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

- A variável `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` O módulo chama o serviço de autor do AEM para encontrar os detalhes da modificação.

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

  Consulte [Tutorial de credenciais de serviço do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) para saber mais sobre isso. Além disso, a variável [Arquivos de configuração do App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) para gerenciar segredos e parâmetros de ação.

- A variável `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` O módulo armazena os dados do evento original, a mensagem de atividade e os detalhes de modificação (se houver) no armazenamento da Adobe I/O Runtime.

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

Por fim, atualize o código de ação em `src/dx-excshell-1/actions/aem-event-processor/index.js` para usar os módulos desenvolvidos.

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

- A variável `src/dx-excshell-1/actions/model` pasta contém `aemEvent.js` e `errors.js` arquivos, que são usados pela ação para analisar o evento recebido e manipular erros, respectivamente.
- A variável `src/dx-excshell-1/actions/load-processed-aem-events` contém o código de ação, essa ação é usada pelo SPA para carregar os Eventos AEM processados do armazenamento da Adobe I/O Runtime.
- A variável `src/dx-excshell-1/web-src` contém o código SPA, que exibe os Eventos AEM processados.
- A variável `src/dx-excshell-1/ext.config.yaml` O arquivo contém a configuração e os parâmetros da ação.

## Conceito e principais pontos

Os requisitos de processamento de eventos diferem de projeto para projeto. No entanto, os principais argumentos deste exemplo são:

- O processamento do evento pode ser feito usando a Ação do Adobe I/O Runtime.
- A Ação em tempo de execução pode se comunicar com sistemas como seus aplicativos internos, soluções de terceiros e soluções de Adobe.
- A ação de tempo de execução serve como ponto de entrada para um processo de negócios projetado em torno de uma alteração de conteúdo.





