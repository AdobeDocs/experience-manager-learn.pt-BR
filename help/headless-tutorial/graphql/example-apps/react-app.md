---
title: Aplicativo React - Exemplo AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando APIs AEM GraphQL usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-11-09T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 6%

---

# Aplicativo React{#react-app}

Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando APIs AEM GraphQL usando consultas persistentes. O AEM Headless Client para JavaScript é usado para executar as consultas persistentes do GraphQL que alimentam o aplicativo.

![Aplicativo React com AEM Headless](./assets/react-app/react-app.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [tutorial passo a passo completo](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR) descrever como este aplicativo React foi criado está disponível.

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=11)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo React funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem o [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) a ser instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=pt-BR)
+ Configuração local usando [o AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR?lang=pt#install-local-aem-instances)

O aplicativo React foi projetado para se conectar a um __AEM Publish__ ambiente, no entanto, ele poderá obter conteúdo do Autor do AEM se a autenticação for fornecida na configuração do aplicativo React.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd-graphql/react-app/.env.development` arquivo e definir `REACT_APP_HOST_URI` para apontar para seu AEM alvo.

   Atualize o método de autenticação se estiver se conectando a uma instância de autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
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

1. Uma nova janela do navegador deve ser carregada em [http://localhost:3000](http://localhost:3000)
1. Uma lista de aventuras do site de referência WKND deve ser exibida no aplicativo.

## O código

Abaixo está um resumo de como o aplicativo React é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo React usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventura da visualização inicial.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura de `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente possibilita as exibições de detalhes de aventura.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

As consultas persistentes de AEM são executadas por HTTP GET e, portanto, o [Cliente AEM Headless para JavaScript](https://github.com/adobe/aem-headless-client-js) é usado para [executar as consultas persistentes do GraphQL](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) contra o AEM e carregue o conteúdo de aventura no aplicativo.

Cada consulta persistente tem um React correspondente [useEffect](https://reactjs.org/docs/hooks-effect.html) gancho em `src/api/usePersistedQueries.js`, que chama de forma assíncrona o ponto de acesso de consulta persistente do AEM HTTP GET e retorna os dados de aventura.

Cada função, por sua vez, chama a variável `aemHeadlessClient.runPersistedQuery(...)`, executando a consulta persistente do GraphQL.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity) {
  ...
  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    const queryParameters = { activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryParameters);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all");
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

   Chamadas `getAdventuresByActivity(..)` de `src/api/usePersistedQueries.js` e exibe as aventuras retornadas em uma lista.

+ `src/components/AdventureDetail.js`

   Chama o `getAdventureBySlug(..)` usando o `slug` parâmetro passado pela seleção de aventura no `Adventures` e exibe os detalhes de uma única aventura.

### Variáveis de ambiente

Vários [variáveis de ambiente](https://create-react-app.dev/docs/adding-custom-environment-variables) são usados para se conectar a um ambiente AEM. O padrão se conecta ao AEM Publish em execução em `http://localhost:4503`. Atualize o `.env.development` arquivo, para alterar a conexão AEM:

+ `REACT_APP_HOST_URI=http://localhost:4502`: Definir para host de destino do AEM
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Defina o caminho do endpoint do GraphQL. Isso não é usado por este aplicativo React, pois este aplicativo usa apenas consultas persistentes.
+ `REACT_APP_AUTH_METHOD=`: o método de autenticação preferencial. Opcional, como por padrão, nenhuma autenticação é usada.
   + `service-token`: usar credenciais de serviço para obter um token de acesso no AEM as a Cloud Service
   + `dev-token`: Use o token de desenvolvimento para desenvolvimento local no AEM as a Cloud Service
   + `basic`: use o usuário/senha para desenvolvimento local com o AEM Author local
   + Deixe em branco para se conectar ao AEM sem autenticação
+ `REACT_APP_AUTHORIZATION=admin:admin`: defina credenciais básicas de autenticação para usar se estiver se conectando a um ambiente de autor do AEM (somente para desenvolvimento). Se você estiver se conectando a um Ambiente de publicação, essa configuração não será necessária.
+ `REACT_APP_DEV_TOKEN`: cadeia de caracteres do token de desenvolvimento. Para se conectar à instância remota, além da autenticação básica (user:pass), você pode usar a autenticação de portador com o token DEV do console da nuvem
+ `REACT_APP_SERVICE_TOKEN`: Caminho para o arquivo de credenciais de serviço. Para se conectar à instância remota, a autenticação também pode ser feita com o token de serviço (baixe o arquivo do Console do desenvolvedor).

### Solicitações de Proxy AEM

Ao usar o servidor de desenvolvimento de webpack (`npm start`) o projeto depende de um [configuração de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) usar `http-proxy-middleware`. O arquivo está configurado em [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) e depende de várias variáveis de ambiente personalizadas definidas em `.env` e `.env.development`.

Se você se conectar a um ambiente de autor de AEM, a variável [o método de autenticação precisa ser configurado](#environment-variables).

### Compartilhamento de recursos entre origens (CORS)

Esse aplicativo React depende de uma configuração CORS baseada em AEM sendo executada no ambiente AEM de destino e presume que o aplicativo React é executado em `http://localhost:3000` no modo de desenvolvimento. A variável [Configuração do CORS](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) faz parte da [Site da WKND](https://github.com/adobe/aem-guides-wknd).

![Configuração do CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
