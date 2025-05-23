---
title: Geração de imagens OpenAI por meio de uma extensão personalizada do Console de fragmentos de conteúdo
description: Saiba como gerar imagens digitais a partir de descrições em linguagem natural usando OpenAI ou DALL·E 2 e faz upload das imagens geradas para o AEM usando uma extensão personalizada do Console de fragmentos de conteúdo.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11649
thumbnail: KT-11649.png
doc-type: article
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: f3047f1d-1c46-4aee-9262-7aab35e9c4cb
duration: 1438
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1289'
ht-degree: 0%

---

# Geração de ativos de imagem do AEM usando OpenAI

Saiba como gerar uma imagem usando OpenAI ou DALL·E 2 e carregá-la no AEM DAM para velocidade de conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/3413093?quality=12&learn=on)

Este exemplo de extensão do Console do Fragmento de Conteúdo do AEM é uma extensão da [barra de ação](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) que gera imagem digital a partir da entrada de linguagem natural usando a [API OpenAI](https://openai.com/api/) ou o [DALL·E 2](https://openai.com/dall-e-2/). A imagem gerada é carregada no DAM do AEM e a propriedade de imagem do fragmento de conteúdo selecionado é atualizada para fazer referência a essa imagem recém-gerada e carregada do DAM.

Neste exemplo, você aprenderá:

1. Geração de imagem usando a [API OpenAI](https://beta.openai.com/docs/guides/images/image-generation-beta) ou a [DALL·E 2](https://openai.com/dall-e-2/)
2. Upload de imagens no AEM
3. Atualização da propriedade do fragmento de conteúdo

O fluxo funcional da extensão de exemplo é o seguinte:

![Fluxo de ação do Adobe I/O Runtime para geração de imagem digital](./assets/digital-image-generation/flow.png){align="center"}

1. Selecione o Fragmento do conteúdo e, clicando no botão `Generate Image` da extensão na [barra de ações](#extension-registration), será aberto o [modal](#modal).
1. O [modal](#modal) exibe um formulário de entrada personalizado criado com o [Espectro de Reação](https://react-spectrum.adobe.com/react-spectrum/).
1. O envio do formulário envia o texto `Image Description` fornecido pelo usuário, o fragmento de conteúdo selecionado e o host do AEM para a [ação personalizada de Adobe I/O Runtime](#adobe-io-runtime-action).
1. A [ação do Adobe I/O Runtime](#adobe-io-runtime-action) valida as entradas.
1. Em seguida, ele chama a API [Image generation](https://beta.openai.com/docs/guides/images/image-generation-beta) do OpenAI e usa o texto `Image Description` para especificar qual imagem deve ser gerada.
1. O ponto de extremidade [geração de imagem](https://beta.openai.com/docs/guides/images/image-generation-beta) cria uma imagem original de _1024x1024_ pixels usando o valor do parâmetro de solicitação de prompt e retorna a URL de imagem gerada como resposta.
1. A [ação do Adobe I/O Runtime](#adobe-io-runtime-action) baixa a imagem gerada para o tempo de execução do App Builder.
1. Em seguida, ele inicia o upload da imagem do tempo de execução do App Builder para o AEM DAM em um caminho predefinido.
1. O AEM as a Cloud Service salva a imagem no DAM e retorna respostas de sucesso ou falha à ação do Adobe I/O Runtime. A resposta de upload bem-sucedido atualiza o valor da propriedade de imagem do fragmento de conteúdo selecionado usando outra solicitação HTTP para o AEM a partir da ação do Adobe I/O Runtime.
1. O modal recebe a resposta da ação do Adobe I/O Runtime e fornece o link de detalhes do ativo do AEM da imagem recém-gerada e carregada.

## Ponto de extensão

Este exemplo estende o ponto de extensão `actionBar` para adicionar um botão personalizado ao Console de fragmentos de conteúdo.

| Interface do usuário estendida do AEM | Ponto de extensão |
| ------------------------ | --------------------- | 
| [Console de fragmentos de conteúdo](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [Barra de Ações](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |

## Exemplo de extensão

O exemplo usa um projeto Adobe Developer Console existente e as seguintes opções ao inicializar o aplicativo App Builder via `aio app init`.

+ Quais modelos você deseja pesquisar?: `All Extension Points`
+ Escolha o(s) modelo(s) para instalar:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Como você deseja nomear sua extensão?: `Image generation`
+ Forneça uma breve descrição da sua extensão: `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ Com qual versão você gostaria de começar?: `0.0.1`
+ O que você gostaria de fazer a seguir?
   + `Add a custom button to Action Bar`
      + Forneça o nome do rótulo para o botão: `Generate Image`
      + Você precisa mostrar uma modal para o botão? `y`
   + `Add server-side handler`
      + O Adobe I/O Runtime permite chamar o código sem servidor sob demanda. Como gostaria de nomear esta ação?: `generate-image`

O aplicativo de extensão do App Builder gerado é atualizado conforme descrito abaixo.

### Configuração inicial

1. Inscreva-se para obter uma conta gratuita da [API do OpenAI](https://openai.com/api/) e crie uma [Chave de API](https://beta.openai.com/account/api-keys)
1. Adicione esta chave ao arquivo `.env` do projeto do App Builder

   ```
       # Specify your secrets here
       # This file must not be committed to source control
       ## Adobe I/O Runtime credentials
       ...
       AIO_runtime_apihost=https://adobeioruntime.net
       ...
       # OpenAI secret API key
       OPENAI_API_KEY=my-openai-secrete-key-to-generate-images
       ...
   ```

1. Passar `OPENAI_API_KEY` como parâmetro para a ação do Adobe I/O Runtime, atualizar o `src/aem-cf-console-admin-1/ext.config.yaml`

   ```yaml
       ...
   
       runtimeManifest:
         packages:
           aem-cf-console-admin-1:
             license: Apache-2.0
             actions:
               generate-image:
                 function: actions/generate-image/index.js
                 web: 'yes'
                 runtime: nodejs:16
                 inputs:
                   LOG_LEVEL: debug
                   OPENAI_API_KEY: $OPENAI_API_KEY
       ...
   ```

1. Instale as bibliotecas Node.js abaixo
   1. [A biblioteca OpenAI Node.js](https://github.com/openai/openai-node#installation) - para invocar a API OpenAI facilmente
   1. [Carregamento do AEM](https://github.com/adobe/aem-upload#install) - para carregar imagens para instâncias do AEM-CS.


>[!TIP]
>
>Nas seções a seguir, você aprenderá sobre os principais arquivos de ação do React e do Adobe I/O Runtime no JavaScript. Para sua referência, os arquivos de chave da pasta `web-src` e `actions` do projeto AppBuilder são fornecidos, consulte [adobe-appbuilder-cfc-ext-image-generation-code.zip](./assets/digital-image-generation/adobe-appbuilder-cfc-ext-image-generation-code.zip).


### Rotas de aplicativo{#app-routes}

O `src/aem-cf-console-admin-1/web-src/src/components/App.js` contém o [roteador React](https://reactrouter.com/en/main).

Há dois conjuntos lógicos de rotas:

1. A primeira rota mapeia solicitações para `index.html`, que invoca o componente React responsável pelo [registro de extensão](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. O segundo conjunto de rotas mapeia URLs para componentes do React que renderizam o conteúdo do modal da extensão. O parâmetro `:selection` representa um caminho de fragmento de conteúdo de lista delimitada.

   Se a extensão tiver vários botões para invocar ações distintas, cada [registro de extensão](#extension-registration) mapeará para uma rota definida aqui.

   ```javascript
   <Route
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
       />
   ```

### Registro de extensão

`ExtensionRegistration.js`, mapeado para a rota `index.html`, é o ponto de entrada para a extensão do AEM e define:

1. O local do botão de extensão aparece na experiência de criação do AEM (`actionBar` ou `headerMenu`)
1. A definição do botão de extensão na função `getButtons()`
1. O manipulador de cliques para o botão, na função `onClick()`

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Some unique ID for the extension used to facilitate communication between the extension and Content Fragment Console
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButtons() {
            return [{
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck',      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
              // Click handler for the extension button
              onClick(selections) {
                // Collect the selected content fragment paths 
                const selectionIds = selections.map(selection => selection.id);

                // Create a URL that maps to the 
                const modalURL = "/index.html#" + generatePath(
                  "/content-fragment/:selection/generate-image-modal",
                  {
                    // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                    selection: encodeURIComponent(selectionIds.join('|')),
                  }
                );

                // Open the route in the extension modal using the constructed URL
                guestConnection.host.modal.showUrl({
                  title: "Generate Image",
                  url: modalURL
                })
                },
              },
            ];
          },
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;          
```

### Modal

Cada rota da extensão, conforme definido em [`App.js`](#app-routes), é mapeada para um componente React renderizado no modal da extensão.

Neste aplicativo de exemplo, há um componente modal do React (`GenerateImageModal.js`) com quatro estados:

1. Carregando, indicando que o usuário deve aguardar
1. A mensagem de aviso que sugere que os usuários selecionem apenas um fragmento de conteúdo por vez
1. O formulário Gerar imagem que permite ao usuário fornecer uma descrição da imagem no idioma natural.
1. A resposta da operação de geração de imagem, fornecendo o link de detalhes do ativo do AEM da imagem recém-gerada e carregada.

É importante observar que qualquer interação com o AEM a partir da extensão deve ser delegada a uma [ação Adobe I/O Runtime do AppBuilder](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), que é um processo separado sem servidor em execução no [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
O uso de ações do Adobe I/O Runtime para se comunicar com o AEM é para evitar problemas de conectividade com o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens).

Quando o formulário _Gerar Imagem_ é enviado, um `onSubmitHandler()` personalizado invoca a ação do Adobe I/O Runtime, transmitindo a descrição da imagem, o host (domínio) do AEM atual e o token de acesso do AEM do usuário. A ação chama a API [Image generation](https://beta.openai.com/docs/guides/images/image-generation-beta) do OpenAI para gerar uma imagem usando a descrição da imagem enviada. Em seguida, usando a classe `DirectBinaryUpload` do módulo de nó [Upload do AEM](https://github.com/adobe/aem-upload), ele carrega a imagem gerada para o AEM e, por fim, usa a [API de Fragmento de Conteúdo do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html?lang=pt-BR) para atualizar os fragmentos de conteúdo.

Quando a resposta da ação Adobe I/O Runtime é recebida, o modal é atualizado para exibir os resultados da operação de geração de imagem.

+ `src/aem-cf-console-admin-1/web-src/src/components/GenerateImageModal.js`

```javascript
export default function GenerateImageModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the application state
  const [imageDescription, setImageDescription] = useState(null);
  const [validationState, setValidationState] = useState({});

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Get the selected content fragment paths from the route parameter `:selection`
  const { selection } = useParams();
  const fragmentIds = selection?.split('|') || [];

  console.log('Selected Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error('The Content Fragments are not selected, can NOT generate images');
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />;
  } if (actionInvokeInProgress) {
    // If the 'Generate Image' action has been invoked but not completed, display a loading spinner
    return <Spinner />;
  } if (fragmentIds.length > 1) {
    // If more than one CF selected show warning and suggest to select only one CF
    return renderMoreThanOneCFSelectionError();
  } if (fragmentIds.length === 1 && !actionResponse) {
    // Display the 'Generate Image' modal and ask for image description
    return renderImgGenerationForm();
  } if (actionResponse) {
    // If the 'Generate Image' action has completed, display the response
    return renderActionResponse();
  }

  /**
   * Renders the message suggesting to select only on CF at a time to not lose credits accidentally
   *
   * @returns the suggestion or error message to select one CF at a time
   */
  function renderMoreThanOneCFSelectionError() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">
          <Text>
            As this operation
            <strong> uses credits from Generative AI services</strong>
            {' '}
            such as DALL·E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
            <p />
            <strong>So please select only one Content Fragment at this moment.</strong>
          </Text>

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Renders the form asking for image description in the natural language and
   * displays message this action uses credits from Generative AI services.
   *
   * @returns the image description input field and credit usage message
   */
  function renderImgGenerationForm() {
    return (

      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          <Flex width="100%">
            <Form
              width="100%"
            >
              <TextField
                label="Image Description"
                description="The image description in natural language, for e.g. Alaskan adventure in wilderness, animals, and flowers."
                isRequired
                validationState={validationState?.propertyName}
                onChange={setImageDescription}
                contextualHelp={(
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>
                        The
                        <strong>description of an image</strong>
                        {' '}
                        you are looking for in the natural language, for e.g. &quot;Family vacation on the beach with blue ocean, dolphins, boats and drink&quot;
                      </Text>
                    </Content>
                  </ContextualHelp>
                  )}
              />

              <Text>
                <p />
                Please note this will use credits from Generative AI services such as OpenAI/DALL·E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
              </Text>

              <ButtonGroup align="end">
                <Button variant="accent" onPress={onSubmitHandler}>Use Credits</Button>
                <Button variant="accent" onPress={() => guestConnection.host.modal.close()}>Close</Button>
              </ButtonGroup>
            </Form>
          </Flex>

        </Content>
      </Provider>
    );
  }

  function buildAssetDetailsURL(aemImgURL) {
    const urlParts = aemImgURL.split('.com');
    const aemAssetDetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetDetailsURL;
  }

  /**
   * Displays the action response received from the App Builder
   *
   * @returns Displays App Builder action and details
   */
  function renderActionResponse() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          {actionResponse.status === 'success'
            && (
              <>
                <Heading level="4">
                  Successfully generated an image, uploaded it to this AEM-CS Author service, and associated it to the selected Content Fragment.
                </Heading>

                <Text>
                  {' '}
                  Please see generated image in AEM-CS
                  {' '}
                  <Link>
                    <a href={buildAssetDetailsURL(actionResponse.aemImgURL)} target="_blank" rel="noreferrer">
                      here.
                    </a>
                  </Link>
                </Text>
              </>
            )}

          {actionResponse.status === 'failure'
            && (
            <Heading level="4">
              Failed to generate, upload image, please check App Builder logs.
            </Heading>
            )}

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Handle the Generate Image form submission.
   * This function calls the supporting Adobe I/O Runtime actions such as
   * - Call the Generative AI service (DALL·E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragment's image reference property to use this new image
   *
   * When invoking the Adobe I/O Runtime actions, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The Content Fragment path to update
   *
   * @returns In case of success the updated content fragment, otherwise failure message
   */
  async function onSubmitHandler() {
    console.log('Started Image Generation orchestration');

    // Validate the form input fields
    if (imageDescription?.length > 1) {
      setValidationState({ imageDescription: 'valid' });
    } else {
      setValidationState({ imageDescription: 'invalid' });
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      Authorization: `Bearer ${guestConnection.sharedContext.get('auth').imsToken}`,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg,
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {

      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentId: fragmentIds[0],
      imageDescription,
    };

    const generateImageAction = 'generate-image';

    try {
      const generateImageActionResponse = await actionWebInvoke(allActions[generateImageAction], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(generateImageActionResponse);

      console.log(`Response from ${generateImageAction}:`, actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e);
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

>[!NOTE]
>
>Na função `buildAssetDetailsURL()`, o valor da variável `aemAssetdetailsURL` presume que o [Unified Shell](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/aem-cloud-service-on-unified-shell.html?lang=pt-BR#overview) está habilitado. Se você desabilitou o Unified Shell, deve remover o `/ui#/aem` do valor da variável.


### Ação do Adobe I/O Runtime

Um aplicativo App Builder de extensão do AEM pode definir ou usar 0 ou muitas ações do Adobe I/O Runtime.
A ação do Adobe Runtime é responsável pelo trabalho que requer a interação com o AEM, o Adobe ou serviços da Web de terceiros.

Neste aplicativo de exemplo, a ação do Adobe I/O Runtime `generate-image` é responsável por:

1. Gerando uma imagem usando o serviço [Geração de imagem da API OpenAI](https://beta.openai.com/docs/guides/images/image-generation-beta)
1. Carregando a imagem gerada na instância do AEM-CS usando a biblioteca [Carregamento do AEM](https://github.com/adobe/aem-upload)
1. Fazer uma solicitação HTTP para a API do fragmento de conteúdo do AEM para atualizar a propriedade de imagem do fragmento de conteúdo.
1. Retornando as informações principais de sucessos e falhas para exibição pelo modal (`GenerateImageModal.js`)


#### Ponto de entrada (`index.js`)

O `index.js` orquestra acima de 1 a 3 tarefas usando os respectivos módulos do JavaScript, ou seja, `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`. Estes módulos e o código associado estão descritos nas próximas [subseções](#image-generation-module---generate-image-using-openaijs).

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL·E 2) and saves generated image to AEM.
 *
 * It leverages following modules
 *  - 'generate-image-using-openai' - To generate an image using OpenAI API
 *  - 'upload-generated-image-to-aem' - To upload the generated image into AEM-CS instance
 *  - 'update-content-fragement' - To update the CF image property with generated image's DAM path
 *
 */

const { Core } = require('@adobe/aio-sdk');
const {
  errorResponse, stringParameters, getBearerToken, checkMissingRequestInputs,
} = require('../utils');

const { generateImageUsingOpenAI } = require('./generate-image-using-openai');

const { uploadGeneratedImageToAEM } = require('./upload-generated-image-to-aem');

const { updateContentFragmentToUseGeneratedImg } = require('./update-content-fragement');

// main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' });

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action');

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params));

    // check for missing request input parameters and headers
    const requiredParams = ['aemHost', 'fragmentId', 'imageDescription'];
    const requiredHeaders = ['Authorization'];
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // extract the user Bearer token from the Authorization header
    const token = getBearerToken(params);

    // Call OpenAI (DALL·E 2) API to generate an image using image description
    const generatedImageURL = await generateImageUsingOpenAI(params);
    logger.info(`Generated image using OpenAI API and url is : ${generatedImageURL}`);

    // Upload the generated image to AEM-CS
    const uploadedImagePath = await uploadGeneratedImageToAEM(params, generatedImageURL, token);
    logger.info(`Uploaded image to AEM, path is: ${uploadedImagePath}`);

    // Update Content Fragment with the newly generated image reference
    const updateContentFragmentPath = await updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, token);
    logger.info(`Updated Content Fragment path is: ${updateContentFragmentPath}`);

    let result;
    if (updateContentFragmentPath) {
      result = {
        status: 'success', message: 'Successfully generated and uploaded image to AEM', genTechServiceImageURL: generatedImageURL, aemImgURL: uploadedImagePath, fragmentPath: updateContentFragmentPath,
      };
    } else {
      result = { status: 'failure', message: 'Failed to generated and uploaded image, please check App Builder logs' };
    }

    const response = {
      statusCode: 200,
      body: result,
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
  } catch (error) {
    // log any server errors
    logger.error(error);
    // return with 500
    return errorResponse(500, 'server error', logger);
  }
}

exports.main = main;
```

#### Geração de imagem

Este módulo é responsável por chamar o ponto de extremidade [Geração de imagem](https://beta.openai.com/docs/guides/images/image-generation-beta) do OpenAI usando a biblioteca [openai](https://github.com/openai/openai-node). Para obter a chave secreta da API OpenAI definida no arquivo `.env`, ele usa `params.OPENAI_API_KEY`.

+ `src/aem-cf-console-admin-1/actions/generate-image/generate-image-using-openai.js`

```javascript
/**
 * This module calls OpenAI API to generate an image based on image description provided to Action
 *
 */

const { Configuration, OpenAIApi } = require('openai');

const { Core } = require('@adobe/aio-sdk');

// Placeholder than actual OpenAI Image
const PLACEHOLDER_IMG_URL = 'https://www.gstatic.com/webp/gallery/2.png';

async function generateImageUsingOpenAI(params) {
  // create a Logger
  const logger = Core.Logger('generateImageUsingOpenAI', { level: params.LOG_LEVEL || 'info' });

  let generatedImageURL = PLACEHOLDER_IMG_URL;

  // create configuration object with the API Key
  const configuration = new Configuration({
    apiKey: params.OPENAI_API_KEY,
  });

  // create OpenAIApi object
  const openai = new OpenAIApi(configuration);

  logger.info(`Generating image for input: ${params.imageDescription}`);

  try {
    // invoke createImage method with details
    const response = await openai.createImage({
      prompt: params.imageDescription,
      n: 1,
      size: '1024x1024',
    });

    generatedImageURL = response.data.data[0].url;

    logger.info(`The OpenAI generate image url is: ${generatedImageURL}`);
  } catch (error) {
    logger.error(`Error while generating image, details are: ${error}`);
  }

  return generatedImageURL;
}

module.exports = {
  generateImageUsingOpenAI,
};
```

#### Fazer upload para o AEM

Este módulo é responsável pelo upload da imagem gerada pelo OpenAI no AEM usando a biblioteca [AEM Upload](https://github.com/adobe/aem-upload). A imagem gerada é baixada primeiro para o tempo de execução do App Builder AEM usando a biblioteca [Sistema de Arquivos](https://nodejs.org/api/fs.html) da Node.js e, uma vez concluída a transferência, ela é excluída.

No código abaixo, a função `uploadGeneratedImageToAEM` orquestra o download da imagem gerada para o tempo de execução, carrega essa imagem para a AEM e a exclui do tempo de execução. A imagem é carregada no caminho `/content/dam/wknd-shared/en/generated`. Verifique se todas as pastas existem no DAM e se ele é um pré-requisito para usar a biblioteca [AEM Upload](https://github.com/adobe/aem-upload).

+ `src/aem-cf-console-admin-1/actions/generate-image/upload-generated-image-to-aem.js`

```javascript
/**
 * This module uploads the generated image to AEM-CS instance using current user's IMS token
 *
 */

const { Core } = require('@adobe/aio-sdk');
const fs = require('fs');

const {
  DirectBinaryUploadErrorCodes,
  DirectBinaryUpload,
  DirectBinaryUploadOptions,
} = require('@adobe/aem-upload');

const codes = DirectBinaryUploadErrorCodes;
const IMG_EXTENSION = '.png';

const GENERATED_IMAGES_DAM_PATH = '/content/dam/wknd-shared/en/generated';

async function downloadImageToRuntime(logger, generatedImageURL) {
  logger.log('Downloading generated image to the runtime');

  // placeholder image name
  let generatedImageName = 'generated.png';

  try {
    // Get the generated image name from the image URL
    const justImgURL = generatedImageURL.substring(0, generatedImageURL.indexOf(IMG_EXTENSION) + 4);
    generatedImageName = justImgURL.substring(justImgURL.lastIndexOf('/') + 1);

    // Read image from URL as the buffer
    const response = await fetch(generatedImageURL);
    const buffer = await response.buffer();

    // Write/download image to the runtime
    fs.writeFileSync(generatedImageName, buffer, (err) => {
      if (err) throw err;
      logger.log('Saved the generated image!');
    });
  } catch (error) {
    logger.error(`Error while downloading image on the runtime, details are: ${error}`);
  }

  return generatedImageName;
}

function setupEventHandlers(binaryUpload, logger) {
  binaryUpload.on('filestart', (data) => {
    const { fileName } = data;

    logger.log(`Started file upload ${fileName}`);
  });

  binaryUpload.on('fileprogress', (data) => {
    const { fileName, transferred } = data;

    logger.log(`Fileupload is in progress ${fileName} & ${transferred}`);
  });

  binaryUpload.on('fileend', (data) => {
    const { fileName } = data;

    logger.log(`Finished file upload ${fileName}`);
  });

  binaryUpload.on('fileerror', (data) => {
    const { fileName, errors } = data;

    logger.log(`Error in file upload ${fileName} and ${errors}`);
  });
}

async function getImageSize(downloadedImgName) {
  const stats = fs.statSync(downloadedImgName);
  return stats.size;
}

async function uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken) {
  let aemImageURL;
  try {
    logger.log('Uploading generated image to AEM from the runtime');

    const binaryUpload = new DirectBinaryUpload();

    // setup event handlers to track the progress, success or error
    setupEventHandlers(binaryUpload, logger);

    // get downloaded image size
    const imageSize = await getImageSize(downloadedImgName);
    logger.info(`The image upload size is: ${imageSize}`);

    // The deatils of the file to be uploaded
    const uploadFiles = [
      {
        fileName: downloadedImgName, // name of the file as it will appear in AEM
        fileSize: imageSize, // total size, in bytes, of the file
        filePath: downloadedImgName, // Full path to the local file
      },
    ];

    // Provide AEM URL and DAM Path where images will be uploaded
    const options = new DirectBinaryUploadOptions()
      .withUrl(`${aemURL}${GENERATED_IMAGES_DAM_PATH}`)
      .withUploadFiles(uploadFiles);

    // Add headers like content type and authorization
    options.withHeaders({
      'content-type': 'image/png',
      Authorization: `Bearer ${accessToken}`,
    });

    // Start the upload to AEM
    await binaryUpload.uploadFiles(options)
      .then((result) => {
        // Handle Error
        result.getErrors().forEach((error) => {
          if (error.getCode() === codes.ALREADY_EXISTS) {
            logger.error('The generated image already exists');
          }
        });

        // Handle Upload result and check for errors
        result.getFileUploadResults().forEach((fileResult) => {
          // log file upload result
          logger.info(`File upload result ${JSON.stringify(fileResult)}`);

          fileResult.getErrors().forEach((fileErr) => {
            if (fileErr.getCode() === codes.ALREADY_EXISTS) {
              const fileName = fileResult.getFileName();
              logger.error(`The generated image already exists ${fileName}`);
            }
          });
        });
      })
      .catch((err) => {
        logger.info(`Failed to uploaded generated image to AEM${err}`);
      });

    logger.info('Successfully uploaded generated image to AEM');

    aemImageURL = `${aemURL + GENERATED_IMAGES_DAM_PATH}/${downloadedImgName}`;
  } catch (error) {
    logger.info(`Error while uploading generated image to AEM, see ${error}`);
  }

  return aemImageURL;
}

async function deleteFileFromRuntime(logger, downloadedImgName) {
  try {
    logger.log('Deleting the generated image from the runtime');

    fs.unlinkSync(downloadedImgName);

    logger.log('Successfully deleted the generated image from the runtime');
  } catch (error) {
    logger.error(`Error while deleting generated image from the runtime, details are: ${error}`);
  }
}

async function uploadGeneratedImageToAEM(params, generatedImageURL, accessToken) {
  // create a Logger
  const logger = Core.Logger('uploadGeneratedImageToAEM', { level: params.LOG_LEVEL || 'info' });

  const aemURL = params.aemHost;

  logger.info(`Uploading generated image from ${generatedImageURL} to AEM ${aemURL} by streaming the bytes.`);

  // download image to the App Builder runtime
  const downloadedImgName = await downloadImageToRuntime(logger, generatedImageURL);

  // Upload image to AEM from the App Builder runtime
  const aemImageURL = await uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken);

  // Delete the downloaded image from the App Builder runtime
  await deleteFileFromRuntime(logger, downloadedImgName);

  return aemImageURL;
}

module.exports = {
  uploadGeneratedImageToAEM,
};
```

#### Atualizar fragmento de conteúdo

Esse módulo é responsável por atualizar a propriedade de imagem do fragmento de conteúdo específico com o caminho DAM da imagem recém-carregada usando a API de fragmento de conteúdo do AEM.

+ `src/aem-cf-console-admin-1/actions/generate-image/update-content-fragement.js`

```javascript
/**
 * This module updates the CF image property with generated image's DAM path
 *
 */
const { Core } = require('@adobe/aio-sdk');

const ADVENTURE_MODEL_IMG_PROPERTY_NAME = 'primaryImage';

const ARTICLE_MODEL_IMG_PROPERTY_NAME = 'featuredImage';

const AUTHOR_MODEL_IMG_PROPERTY_NAME = 'profilePicture';

function findImgPropertyName(fragmenPath) {
  if (fragmenPath && fragmenPath.includes('/adventures')) {
    return ADVENTURE_MODEL_IMG_PROPERTY_NAME;
  } if (fragmenPath && fragmenPath.includes('/magazine')) {
    return ARTICLE_MODEL_IMG_PROPERTY_NAME;
  }
  return AUTHOR_MODEL_IMG_PROPERTY_NAME;
}

async function updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, accessToken) {
  // create a Logger
  const logger = Core.Logger('updateContentFragment', { level: params.LOG_LEVEL || 'info' });

  const fragmenPath = params.fragmentId;
  const imgPropName = findImgPropertyName(fragmenPath);
  const relativeImgPath = uploadedImagePath.substring(uploadedImagePath.indexOf('/content/dam'));

  logger.info(`Update CF ${fragmenPath} to use ${relativeImgPath} image path`);

  const body = {
    properties: {
      elements: {
        [imgPropName]: {
          value: relativeImgPath,
        },
      },
    },
  };

  const res = await fetch(`${params.aemHost}${fragmenPath.replace('/content/dam/', '/api/assets/')}.json`, {
    method: 'put',
    body: JSON.stringify(body),
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },

  });

  if (res.ok) {
    logger.info(`Successfully updated ${fragmenPath}`);
    return fragmenPath;
  }

  logger.info(`Failed to update ${fragmenPath}`);
  return '';
}

module.exports = {
  updateContentFragmentToUseGeneratedImg,
};
```
