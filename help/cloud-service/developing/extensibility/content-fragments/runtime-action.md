---
title: AEM Extensão do console Fragmento do conteúdo Ações do Adobe I/O Runtime
description: Saiba como criar um modal de extensão do console Fragmento de conteúdo AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 1%

---


# Ação do Adobe I/O Runtime

![AEM ações de tempo de execução da extensão de fragmento de conteúdo](./assets/runtime-action/action-runtime-flow.png){align="center"}

AEM As extensões de Fragmento de conteúdo podem incluir, opcionalmente, qualquer número de [Ações do Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).

As ações Adobe I/O Runtime são funções sem servidor que podem ser chamadas pela extensão. As ações são úteis para executar um trabalho que requer interação com AEM ou outros serviços da Web do Adobe.
As ações normalmente são mais úteis para executar tarefas de longa duração (qualquer coisa maior do que alguns segundos) ou fazer solicitações HTTP para AEM ou outros serviços da Web.

Os benefícios de usar as ações do Adobe I/O Runtime para executar o trabalho são:

+ As ações são funções sem servidor que são executadas fora do contexto de um navegador, removendo a necessidade de se preocupar com o CORS
+ As ações não podem ser interrompidas pelo usuário (por exemplo, atualizar o navegador)
+ As ações são assíncronas, para que possam ser executadas enquanto necessário sem bloquear o usuário

No contexto de extensões de Fragmento de conteúdo AEM, as ações são usadas com mais frequência para se comunicar diretamente com AEM as a Cloud Service, com frequência:

+ Coleta de dados relacionados do AEM sobre os Fragmentos de conteúdo
+ Execução de operações personalizadas em Fragmentos de conteúdo
+ Criar fragmentos de conteúdo com base em

Embora a extensão Fragmento de conteúdo AEM seja exibida no console Fragmento de conteúdo, nas extensões e nas ações de suporte, é possível invocar qualquer API HTTP AEM disponível, incluindo pontos de extremidade de API AEM personalizados.

## Chamar uma ação

As ações do Adobe I/O Runtime são disparadas principalmente de dois lugares em uma extensão de Fragmento de conteúdo AEM:

1. O [do registro de extensões](./extension-registration.md) `onClick(..)` manipulador
1. Dentro de um [modal](./modal.md)

### Do registro de extensão

As ações do Adobe I/O Runtime podem ser chamadas diretamente do código de registro da extensão. O caso de uso mais comum é vincular uma ação a um [menu de cabeçalho](./header-menu.md#no-modal)Botão &#39;s&#39; que não usa [modals](./modal.md).

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
// allActions is an object containing all the actions defined in the extension's manifest
import allActions from '../config.json'

// actionWebInvoke is a helper that invokes an action
import actionWebInvoke from '../utils'
...
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your header menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension',  // Unique ID for the button
              'label': 'My header menu extension',       // Button label 
              'icon': 'Edit'                             // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

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
        }
      }
    })
  }
  init().catch(console.error);
}
```

### Do modal

As ações do Adobe I/O Runtime podem ser chamadas diretamente dos modais para realizar um trabalho mais envolvido, especialmente trabalhos que dependem da comunicação com AEM serviço da Web do Adobe, as a Cloud Service ou até mesmo serviços de terceiros.

As ações do Adobe I/O Runtime são aplicativos JavaScript baseados em Node.js executados no ambiente Adobe I/O Runtime sem servidor. Essas ações são endereçáveis via HTTP pelo SPA de extensão.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

+ `src/aem-cf-console-admin-1/actions/generic/index.js`

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

## AEM APIs HTTP

As seguintes APIs HTTP AEM geralmente são usadas para interagir com AEM das extensões:

+ [AEM APIs do GraphQL](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html)
+ [API HTTP AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html)
   + [Compatibilidade com os Fragmentos de conteúdo na API HTTP do AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/assets-api-content-fragments.html)
+ [AEM API do QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api.html)
+ [Referência completa AEM API as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/reference-materials.html)


## Módulos Adobe npm

Os seguintes são módulos npm úteis para desenvolver ações do Adobe I/O Runtime:

+ [@adobe/aio-sdk](https://www.npmjs.com/package/@adobe/aio-sdk)
   + [SDK principal](https://github.com/adobe/aio-sdk-core)
   + [Biblioteca de estado](https://github.com/adobe/aio-lib-state)
   + [Biblioteca de arquivos](https://github.com/adobe/aio-lib-files)
   + [Biblioteca da Adobe Target](https://github.com/adobe/aio-lib-target)
   + [Biblioteca da Adobe Analytics](https://github.com/adobe/aio-lib-analytics)
   + [Biblioteca da Adobe Campaign Standard](https://github.com/adobe/aio-lib-campaign-standard)
   + [Biblioteca de perfil do cliente do Adobe](https://github.com/adobe/aio-lib-customer-profile)
   + [Biblioteca de dados do cliente da Adobe Audience Manager](https://github.com/adobe/aio-lib-audience-manager-cd)
   + [Adobe I/O Events](https://github.com/adobe/aio-lib-events)
+ [@adobe/aio-lib-core-networking](https://github.com/adobe/aio-lib-core-networking)
+ [@adobe/node-httptransfer](https://github.com/adobe/node-httptransfer)