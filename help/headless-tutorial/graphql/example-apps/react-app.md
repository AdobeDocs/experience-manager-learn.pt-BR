---
title: Aplicativo React - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando APIs GraphQL AEM usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 3ef802d4e87b7dc8132dae25c9dbb801dfdce0fe
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 5%

---

# React App

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando APIs GraphQL AEM usando consultas persistentes. O Cliente Sem Cabeçalho do AEM para JavaScript é usado para executar as consultas persistentes de GraphQL que alimentam o aplicativo.

![Reagir ao aplicativo com AEM headless](./assets/react-app/react-app.png)

Visualize o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [tutorial passo a passo completo](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR) descrevendo como esse aplicativo React foi criado está disponível.

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Requisitos AEM

O aplicativo React funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem o [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) a ser instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR?lang=en#install-local-aem-instances)

O aplicativo React foi projetado para se conectar a um __Publicação do AEM__ , no entanto, ele pode gerar conteúdo do Autor do AEM se a autenticação for fornecida na configuração do aplicativo React.

## Como usar

1. Clonar o `adobbe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd-graphql/react-app/.env.development` arquivo e conjunto `REACT_APP_HOST_URI` para apontar para o seu AEM alvo.

   Atualize o método de autenticação se estiver se conectando a uma instância do autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/_cq_graphql/wknd-shared/endpoint.json
   
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

Abaixo está um resumo de como o aplicativo React é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Consultas persistentes

Seguindo AEM práticas recomendadas headless, o aplicativo React usa consultas persistentes AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventuras da exibição inicial.

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

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura por `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente potencializa as exibições de detalhes da aventura.

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

### Executar consulta persistente de GraphQL

AEM consultas persistentes são executadas pelo HTTP GET e, portanto, o [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js) é usado para [executar as consultas GraphQL persistentes](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) contra AEM e carregue o conteúdo de aventura no aplicativo.

Cada consulta persistente tem uma função correspondente em `src/api/persistedQueries.js`, que chama de forma assíncrona o ponto de extremidade AEM HTTP GET e retorna os dados de aventura.

Cada função, por sua vez, chama a função `aemHeadlessClient.runPersistedQuery(...)`, executando a consulta GraphQL mantida.

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### Exibições

O aplicativo React usa duas visualizações para apresentar os dados de aventura na experiência da Web.

+ `src/components/Adventures.js`

   Invoca `getAllAdventures()` from `src/api/persistedQueries.js`  e exibe as aventuras retornadas em uma lista.

+ `src/components/AdventureDetail.js`

   Invoca o `getAdventureBySlug(..)` usando o `slug` param transmitido por meio da seleção de aventura no `Adventures` e exibe os detalhes de uma única aventura.


### Variáveis de ambiente

Vários [variáveis de ambiente](https://create-react-app.dev/docs/adding-custom-environment-variables) são usadas para se conectar a um ambiente de AEM. O padrão se conecta à publicação do AEM em execução em `http://localhost:4503`. Para alterar a conexão AEM, atualize o `.env.development` arquivo:

+ `REACT_APP_HOST_URI=http://localhost:4502`: Definir como AEM host de destino
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Defina o caminho do ponto de extremidade GraphQL. Isso não é usado por este aplicativo React, pois esse aplicativo usa somente consultas persistentes.
+ `REACT_APP_AUTH_METHOD=`: O método de autenticação preferencial. Opcional, como por padrão, nenhuma autenticação é usada.
   + `service-token`: Usar credenciais de serviço para obter um token de acesso em AEM as a Cloud Service
   + `dev-token`: Usar token dev para desenvolvimento local em AEM as a Cloud Service
   + `basic`: Usar usuário/passagem para desenvolvimento local com o autor local do AEM
   + Deixe em branco para se conectar ao AEM sem autenticação
+ `REACT_APP_AUTHORIZATION=admin:admin`: Defina as credenciais básicas de autenticação a serem usadas ao se conectar a um ambiente de Autor do AEM (somente para desenvolvimento). Se estiver se conectando a um ambiente de Publicação, essa configuração não será necessária.
+ `REACT_APP_DEV_TOKEN`: Sequência de token de desenvolvimento. Para se conectar à instância remota, ao lado do Basic auth (user:pass), você pode usar o Bearer auth com o token DEV do console do Cloud
+ `REACT_APP_SERVICE_TOKEN`: Caminho para o arquivo de credenciais de serviço. Para se conectar à instância remota, a autenticação também pode ser feita com o token de serviço (arquivo de download do Console do desenvolvedor).

### Solicitações de AEM de proxy

Ao usar o servidor de desenvolvimento do webpack (`npm start`) o projeto depende de um [configuração de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) usar `http-proxy-middleware`. O arquivo é configurado em [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) e depende de várias variáveis de ambiente personalizadas definidas em `.env` e `.env.development`.

Se estiver se conectando a um ambiente de autor de AEM, a variável [o método de autenticação precisa ser configurado](#environment-variables).

### Compartilhamento de recursos entre origens (CORS)

Esse aplicativo React depende de uma configuração CORS baseada em AEM em execução no ambiente de AEM de destino e assume que o aplicativo React é executado em `http://localhost:3000` em modo de desenvolvimento. O [Configuração do CORS](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) faz parte do [Site WKND](https://github.com/adobe/aem-guides-wknd).

![Configuração do CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
