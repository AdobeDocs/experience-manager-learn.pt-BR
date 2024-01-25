---
title: Exemplo de atualização de propriedade em massa para a extensão do Console do Fragmento de conteúdo do AEM
description: Um exemplo de extensão do console Fragmentos de conteúdo do AEM que atualiza em massa uma propriedade de Fragmentos de conteúdo.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
duration: 1475
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# Extensão de exemplo de atualização de propriedade em massa

>[!VIDEO](https://video.tv.adobe.com/v/3412296?quality=12&learn=on)

Este exemplo de extensão do Console de fragmentos de conteúdo do AEM é um [barra de ação](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) extensão que atualiza em massa uma propriedade Fragmento do conteúdo para um valor comum.

O fluxo funcional da extensão de exemplo é o seguinte:

![Fluxo de ação do Adobe I/O Runtime](./assets/bulk-property-update/flow.png){align="center"}

1. Selecione Fragmentos de conteúdo e clique no botão da extensão na [barra de ação](#extension-registration) abre o [modal](#modal).
2. A variável [modal](#modal) exibe um formulário de entrada personalizado criado com [Espectro React](https://react-spectrum.adobe.com/react-spectrum/).
3. O envio do formulário envia a lista de fragmentos de conteúdo selecionados e o host AEM para o [ação personalizada do Adobe I/O Runtime](#adobe-io-runtime-action).
4. A variável [Ação do Adobe I/O Runtime](#adobe-io-runtime-action) valida as entradas e faz solicitações HTTP PUT ao AEM para atualizar os fragmentos de conteúdo selecionados.
5. Uma série de PUT HTTP para cada Fragmento de conteúdo para atualizar a propriedade especificada.
6. O AEM as a Cloud Service persiste nas atualizações de propriedade do fragmento de conteúdo e retorna respostas de sucesso ou falha à ação do Adobe I/O Runtime.
7. O modal recebeu a resposta da ação do Adobe I/O Runtime e exibe uma lista de atualizações em massa bem-sucedidas.

## Ponto de extensão

Este exemplo se estende ao ponto de extensão `actionBar` para adicionar um botão personalizado ao Console de fragmentos de conteúdo.

| IU do AEM estendida | Ponto de extensão |
| ------------------------ | --------------------- | 
| [Console de fragmentos de conteúdo](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [Barra de ação](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |


## Exemplo de extensão

O exemplo usa um projeto existente do Adobe Developer Console e usa as seguintes opções ao inicializar o aplicativo App Builder, via `aio app init`.

+ Quais modelos você deseja pesquisar?: `All Extension Points`
+ Escolha o(s) modelo(s) para instalar:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Como você deseja nomear sua extensão?: `Bulk property update`
+ Forneça uma breve descrição da sua extensão: `An example action bar extension that bulk updates a single property one or more content fragments.`
+ Com qual versão você gostaria de começar?: `0.0.1`
+ O que você gostaria de fazer a seguir?
   + `Add a custom button to Action Bar`
      + Forneça o nome da etiqueta do botão: `Bulk property update`
      + Você precisa mostrar uma modal para o botão? `y`
   + `Add server-side handler`
      + O Adobe I/O Runtime permite chamar o código sem servidor sob demanda. Como você gostaria de nomear esta ação?: `generic`

O aplicativo de extensão gerado do App Builder é atualizado conforme descrito abaixo.

### Rotas de aplicativo{#app-routes}

A variável `src/aem-cf-console-admin-1/web-src/src/components/App.js` contém o [Roteador React](https://reactrouter.com/en/main).

Há dois conjuntos lógicos de rotas:

1. A primeira rota mapeia solicitações para o `index.html`, que invoca o componente do React responsável pela [registro de extensão](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. O segundo conjunto de rotas mapeia URLs para componentes do React que renderizam o conteúdo do modal da extensão. A variável `:selection` param representa um caminho de fragmento de conteúdo de lista delimitada.

   Se a extensão tiver vários botões para chamar ações distintas, cada um [registro de extensão](#extension-registration) mapeia para uma rota definida aqui.

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

### Registro de extensão

`ExtensionRegistration.js`, mapeado para o `index.html` é o ponto de entrada para a extensão AEM e define:

1. O local do botão de extensão é exibido na experiência de criação do AEM (`actionBar` ou `headerMenu`)
1. A definição do botão de extensão em `getButton()` função
1. O manipulador de cliques do botão, na caixa `onClick()` função

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'bulk-property-update',     // Unique ID for the button
              'label': 'Bulk property update',  // Button label 
              'icon': 'Edit'                    // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/bulk-property-update",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|'))
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Bulk property update",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

### Modal

Cada rota da extensão, conforme definido em [`App.js`](#app-routes), mapeia para um componente React que é renderizado no modal da extensão.

Neste aplicativo de exemplo, há um componente modal do React (`BulkPropertyUpdateModal.js`) que tem três estados:

1. Carregando, indicando que o usuário deve aguardar
1. O formulário Atualização de propriedade em massa que permite que o usuário especifique o nome e o valor da propriedade a ser atualizada
1. A resposta da operação de atualização de propriedade em massa, listando os fragmentos de conteúdo que foram atualizados e aqueles que não puderam ser atualizados

importante, qualquer interação com o AEM a partir da extensão deve ser delegada a um [Ação do AppBuilder Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), que é um processo separado e sem servidor em execução [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
O uso de ações do Adobe I/O Runtime para se comunicar com o AEM é para evitar problemas de conectividade com o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens).

Quando o formulário Atualização de propriedade em massa for enviado, uma solicitação `onSubmitHandler()` invoca a ação do Adobe I/O Runtime, transmitindo o host AEM (domínio) atual e o token de acesso AEM do usuário, que por sua vez chama o [API de fragmento de conteúdo do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) para atualizar os fragmentos de conteúdo.

Quando a resposta da ação do Adobe I/O Runtime é recebida, o modal é atualizado para exibir os resultados da operação de atualização de propriedade em massa.

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


### Ação do Adobe I/O Runtime

Um aplicativo Construtor de aplicativos de extensão AEM pode definir ou usar 0 ou muitas ações do Adobe I/O Runtime.
As ações de Adobe Runtime devem ser um trabalho responsável que requer interação com AEM ou outros serviços da Web de Adobe.

Neste aplicativo de exemplo, a ação Adobe I/O Runtime - que usa o nome padrão `generic` - é responsável por:

1. Fazer uma série de solicitações HTTP para a API do fragmento de conteúdo AEM para atualizar os fragmentos de conteúdo.
1. Coleta das respostas dessas solicitações HTTP, agrupando-as em sucessos e falhas
1. Retornando a lista de sucessos e falhas para exibição pelo modal (`BulkPropertyUpdateModal.js`)

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
