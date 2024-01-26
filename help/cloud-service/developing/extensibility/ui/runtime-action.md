---
title: Ações do Adobe I/O Runtime para extensão da interface do usuário do AEM
description: Saiba como criar um modal de extensão da interface do usuário do AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: 3062900a-0461-4c6f-81e6-c76a7f613804
duration: 220
source-git-commit: 6f1245e804f0311c3f833ea8b2324cbc95272f52
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 1%

---

# Ação do Adobe I/O Runtime

![Ações de tempo de execução de extensão da interface do AEM](./assets/runtime-action/action-runtime-flow.png){align="center"}

As extensões da interface do usuário do AEM podem, opcionalmente, incluir qualquer número de [Ações do Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).

As ações do Adobe I/O Runtime são funções sem servidor que podem ser chamadas pela extensão. As ações são úteis para executar trabalhos que exijam a interação com o AEM ou outros serviços da Web de Adobe. As ações normalmente são mais úteis para executar tarefas de longa duração (algo maior que alguns segundos) ou fazer solicitações HTTP para AEM ou outros serviços da Web.

Os benefícios de usar ações do Adobe I/O Runtime para executar trabalho são:

+ As ações são funções sem servidor executadas fora do contexto de um navegador, eliminando a necessidade de se preocupar com o CORS
+ As ações não podem ser interrompidas pelo usuário (por exemplo, atualizar o navegador)
+ As ações são assíncronas, portanto, podem ser executadas conforme necessário sem bloquear o usuário

No contexto das extensões da interface do usuário do AEM, as ações são frequentemente usadas para se comunicar diretamente com o AEM as a Cloud Service:

+ Coleta de dados relacionados do AEM sobre o conteúdo selecionado ou atual
+ Realização de operações personalizadas no conteúdo
+ Criação de conteúdo sob medida

Embora a extensão da interface do AEM seja exibida em interfaces do usuário AEM específicas, as extensões e suas ações de suporte podem invocar qualquer API HTTP do AEM disponível, incluindo pontos de extremidade da API do AEM personalizados.

## Chamar uma ação

As ações do Adobe I/O Runtime são chamadas principalmente de dois locais em uma extensão da interface do usuário AEM:

1. A variável [do registro de extensão](./extension-registration.md) `onClick(..)` manipulador
1. Em um [modal](./modal.md)

### Do registro de extensão

As ações do Adobe I/O Runtime podem ser chamadas diretamente do código de registro da extensão. O caso de uso mais comum é vincular uma ação a um [menu de cabeçalho](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/)O botão do que não usa [modais](./modal.md).

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
...
// allActions is an object containing all the actions defined in the extension's manifest
import allActions from '../config.json'

// actionWebInvoke is a helper that invokes an action
import actionWebInvoke from '../utils'
...
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension, usually defined in Constants.js
      methods: {
        // Configure your header menu button here
        headerMenu: {
          getButtons() {
            return [{
              'id': 'example.my-header-menu-extension',  // Unique ID for the button
              'label': 'My header menu extension',       // Button label 
              'icon': 'Edit',                             // Button icon from https://spectrum.adobe.com/page/icons/

              // Click handler for the extension button
              onClick() {
                // Set the HTTP headers required to access the Adobe I/O runtime action
                const headers = {
                  'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
                  'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
                };

                // Set the parameters to pass to the Adobe I/O Runtime action
                const params = {
                  aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`, // Pass in the AEM host if the action interacts with AEM
                  aemAccessToken: guestConnection.sharedContext.get('auth').imsToken
                };

                try {
                  // Invoke Adobe I/O Runtime action named `generic`, with the configured headers and parameters.
                  const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);
                } catch (e) {
                  // Log and store any errors
                  console.error(e)
                }           
              }
            }]
          }
        }
      }
    })
  }
  init().catch(console.error);
}

export default ExtensionRegistration;
```

### Do modal

As ações do Adobe I/O Runtime podem ser chamadas diretamente dos modais para realizar trabalhos mais envolvidos, especialmente trabalhos que dependam de comunicação com AEM as a Cloud Service, serviço da web Adobe ou até mesmo serviços de terceiros.

As ações do Adobe I/O Runtime são aplicativos JavaScript baseados em Node.js executados no ambiente Adobe I/O Runtime sem servidor. Essas ações são endereçáveis via HTTP pela extensão SPA.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()
  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (!actionResponse) {
    // Else if the modal is ready to render and has not called the Adobe I/O Runtime action yet
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="cta" onPress={doWork}>Do work</Button>
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  } else {
    // Else the modal has called the Adobe I/O Runtime action and is ready to render the response
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        Done! The response from the action is: { actionResponse }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }

  /**
   * Invoke the Adobe I/O Runtime action and store the response in the React component's state.
   */
  async function doWork() {
    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,
      contentFragmentPaths: contentFragmentPaths
    };

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      // Invoke the Adobe I/O Runtime action named `generic`.
      const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

## Ação do Adobe I/O Runtime

+ `src/aem-ui-extension/actions/generic/index.js`

```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

async function main (params) {
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    logger.debug(stringParameters(params))

    // Check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'contentFragmentPaths' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    // Example HTTP request to AEM payload; This updates all 'title' properties of the Content Fragments to 'Hello World'
    const body = {
      "properties": {
        "elements": {
          "title": {
            "value": "Hello World"
          }
        }
      }
    };

    let results = await Promise.all(params.contentFragmentPaths.map(async (contentFragmentPath) => {
      // Invoke the AEM HTTP Assets Content Fragment API to update each Content Fragment
      // The AEM host is passed in as a parameter to the Adobe I/O Runtime action
      const res = await fetch(`${params.aemHost}${contentFragmentPath.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          // Pass in the accessToken as AEM Author service requires authentication/authorization
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    // Return a response to the AEM Content Fragment extension React application
    const response = {
      statusCode: 200,
      body: results
    };
    return response;

  } catch (error) {
    logger.error(error)
    return errorResponse(500, 'server error', logger)
  }
}
```

## APIs HTTP do AEM

As seguintes APIs HTTP de AEM são normalmente usadas para interagir com AEM a partir de extensões:

+ [APIs do GraphQL para AEM](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=pt-BR)
+ [API HTTP do AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html)
   + [Compatibilidade com os Fragmentos de conteúdo na API HTTP do AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/assets-api-content-fragments.html)
+ [API AEM QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api.html)
+ [Referência completa da API as a Cloud Service do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/reference-materials.html)


## módulos npm Adobe

Estes são módulos npm úteis para o desenvolvimento de ações do Adobe I/O Runtime:

+ [@adobe/aio-sdk](https://www.npmjs.com/package/@adobe/aio-sdk)
   + [SDK principal](https://github.com/adobe/aio-sdk-core)
   + [Biblioteca de estados](https://github.com/adobe/aio-lib-state)
   + [Biblioteca de arquivos](https://github.com/adobe/aio-lib-files)
   + [Biblioteca da Adobe Target](https://github.com/adobe/aio-lib-target)
   + [Biblioteca da Adobe Analytics](https://github.com/adobe/aio-lib-analytics)
   + [Biblioteca da Adobe Campaign Standard](https://github.com/adobe/aio-lib-campaign-standard)
   + [Biblioteca de perfil do cliente do Adobe](https://github.com/adobe/aio-lib-customer-profile)
   + [Biblioteca de dados do cliente da Adobe Audience Manager](https://github.com/adobe/aio-lib-audience-manager-cd)
   + [Eventos Adobe I/O](https://github.com/adobe/aio-lib-events)
+ [@adobe/aio-lib-core-networking](https://github.com/adobe/aio-lib-core-networking)
+ [@adobe/node-httptransfer](https://github.com/adobe/node-httptransfer)
