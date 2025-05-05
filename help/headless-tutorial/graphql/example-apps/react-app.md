---
title: Aplicativo React - Exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
duration: 256
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 1%

---

# Aplicativo React{#react-app}

Aplicativos de exemplo são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes. O AEM Headless Client para JavaScript é usado para executar as consultas persistentes do GraphQL que alimentam o aplicativo.

![Aplicativo React com AEM Headless](./assets/react-app/react-app.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

Um [tutorial passo a passo completo](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR) que descreve como este aplicativo React foi compilado está disponível.

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo React funciona com as seguintes opções de implantação do AEM. Todas as implantações exigem que o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) esteja instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=pt-BR)
+ Configuração local usando o [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
   + Requer [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)

O aplicativo React foi projetado para se conectar a um ambiente de __Publicação do AEM__. No entanto, ele poderá originar conteúdo do Autor do AEM se a autenticação for fornecida na configuração do aplicativo React.

## Como usar

1. Clonar o repositório `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o arquivo `aem-guides-wknd-graphql/react-app/.env.development` e defina `REACT_APP_HOST_URI` para apontar para seu AEM de destino.

   Atualize o método de autenticação se estiver se conectando a uma instância de autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. Abra um terminal e execute os comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Uma nova janela de navegador deve ser carregada em [http://localhost:3000](http://localhost:3000)
1. Uma lista de aventuras do site de referência WKND deve ser exibida no aplicativo.

## O código

Abaixo está um resumo de como o aplicativo React é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo React usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventura da visualização inicial.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura por `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente possibilita as exibições de detalhes de aventura.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Executar consulta persistente do GraphQL

As consultas persistentes do AEM são executadas por HTTP GET e, portanto, o [cliente AEM Headless para JavaScript](https://github.com/adobe/aem-headless-client-js) é usado para [executar as consultas persistentes do GraphQL](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) no AEM e carregar o conteúdo de aventura no aplicativo.

Cada consulta persistente tem um gancho React [useEffect](https://reactjs.org/docs/hooks-effect.html) correspondente em `src/api/usePersistedQueries.js`, que chama de forma assíncrona o ponto de extremidade da consulta persistente do AEM HTTP GET e retorna os dados de aventura.

Cada função, por sua vez, invoca o `aemHeadlessClient.runPersistedQuery(...)`, executando a consulta persistente do GraphQL.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### Exibições

O aplicativo React usa duas visualizações para apresentar os dados de aventura na experiência da web.

+ `src/components/Adventures.js`

  Chama `getAdventuresByActivity(..)` de `src/api/usePersistedQueries.js` e exibe as aventuras retornadas em uma lista.

+ `src/components/AdventureDetail.js`

  Invoca o `getAdventureBySlug(..)` usando o parâmetro `slug` transmitido por meio da seleção de aventura no componente `Adventures`, e exibe os detalhes de uma única aventura.

### Variáveis de ambiente

Várias [variáveis de ambiente](https://create-react-app.dev/docs/adding-custom-environment-variables) são usadas para se conectar a um ambiente AEM. O padrão se conecta ao AEM Publish em execução às `http://localhost:4503`. Atualize o arquivo `.env.development` para alterar a conexão do AEM:

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`: Definir para host de destino do AEM
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Definir o caminho do ponto de extremidade do GraphQL. Isso não é usado por este aplicativo React, pois este aplicativo usa apenas consultas persistentes.
+ `REACT_APP_AUTH_METHOD=`: o método de autenticação preferencial. Opcional, por padrão, nenhuma autenticação é usada.
   + `service-token`: Usar credenciais de serviço para obter um token de acesso no AEM as a Cloud Service
   + `dev-token`: Usar token dev para desenvolvimento local no AEM as a Cloud Service
   + `basic`: Usar usuário/senha para desenvolvimento local com o Autor do AEM local
   + Deixe em branco para se conectar ao AEM sem autenticação
+ `REACT_APP_AUTHORIZATION=admin:admin`: Defina as credenciais básicas de autenticação a serem usadas na conexão com um ambiente de Autor do AEM (somente para desenvolvimento). Se você estiver se conectando a um Ambiente de publicação, essa configuração não será necessária.
+ `REACT_APP_DEV_TOKEN`: Cadeia de caracteres do token de desenvolvimento. Para se conectar à instância remota, além da autenticação básica (user:pass), você pode usar a autenticação de portador com o token DEV do console da nuvem
+ `REACT_APP_SERVICE_TOKEN`: Caminho para o arquivo de credenciais de serviço. Para se conectar à instância remota, a autenticação também pode ser feita com o token de serviço (baixar arquivo da Developer Console).

### Solicitações do Proxy AEM

Ao usar o servidor de desenvolvimento do webpack (`npm start`), o projeto depende de uma [configuração de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) usando `http-proxy-middleware`. O arquivo está configurado em [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) e depende de várias variáveis de ambiente personalizadas definidas em `.env` e `.env.development`.

Se você se conectar a um ambiente de autor do AEM, o [método de autenticação correspondente precisará ser configurado](#environment-variables).

### Compartilhamento de recursos entre origens (CORS)

Este aplicativo React depende de uma configuração CORS com base em AEM em execução no ambiente AEM de destino e presume que o aplicativo React é executado em `http://localhost:3000` no modo de desenvolvimento.  Revise a[documentação de implantação do AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html?lang=pt-BR) para obter mais informações sobre como configurar o CORS.
