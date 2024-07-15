---
title: Next.js - Exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo Next.js demonstra como consultar conteúdo usando as APIs do GraphQL do AEM usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Aplicativo Next.js

Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo Next.js demonstra como consultar conteúdo usando as APIs do GraphQL do AEM usando consultas persistentes. O AEM Headless Client para JavaScript é usado para executar as consultas persistentes do GraphQL que alimentam o aplicativo.

![Aplicativo Next.js com AEM Headless](./assets/next-js/next-js.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo Next.js funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem que o [WKND Compartilhado v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) seja instalado no ambiente do AEM as a Cloud Service.

Este aplicativo Next.js de exemplo foi criado para se conectar ao serviço __AEM Publish__.

### Requisitos do autor do AEM

O Next.js foi criado para se conectar ao serviço __AEM Publish__ e acessar conteúdo desprotegido. O Next.js pode ser configurado para se conectar ao AEM Author por meio das `.env` propriedades descritas abaixo. As imagens veiculadas pelo AEM Author exigem autenticação e, portanto, o usuário que acessar o aplicativo Next.js também deve estar conectado ao AEM Author.

## Como usar

1. Clonar o repositório `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o arquivo `aem-guides-wknd-graphql/next-js/.env.local` e defina `NEXT_PUBLIC_AEM_HOST` para o serviço AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Se você estiver se conectando ao serviço de Autor do AEM, a autenticação deverá ser fornecida, pois o serviço de Autor do AEM é seguro por padrão.

   Para usar um conjunto de contas AEM local `AEM_AUTH_METHOD=basic` e fornecer o nome de usuário e a senha nas propriedades `AEM_AUTH_USER` e `AEM_AUTH_PASSWORD`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Para usar um [token de desenvolvimento local do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token), defina `AEM_AUTH_METHOD=dev-token` e forneça o valor do token de desenvolvimento completo na propriedade `AEM_AUTH_DEV_TOKEN`.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Edite o arquivo `aem-guides-wknd-graphql/next-js/.env.local` e valide se `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` está definido como o ponto de extremidade AEM GraphQL apropriado.

   Ao usar o [Site WKND Compartilhado](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou o [Site WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), use o ponto de extremidade de API do GraphQL `wknd-shared`.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Abra um prompt de comando e inicie o aplicativo Next.js usando os seguintes comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Uma nova janela do navegador abre o aplicativo Next.js em [http://localhost:3000](http://localhost:3000)
1. O aplicativo Next.js exibe uma lista de aventuras. Selecionar uma aventura abre seus detalhes em uma nova página.

## O código

Abaixo está um resumo de como o aplicativo Next.js é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo Next.js usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

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

As consultas persistentes do AEM são executadas por HTTP GET e, portanto, o [cliente AEM Headless do JavaScript](https://github.com/adobe/aem-headless-client-js) é usado para [executar as consultas persistentes do GraphQL AEM](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) em relação ao e carregar o conteúdo de aventura no aplicativo.

Cada consulta persistente tem uma função correspondente em `src/lib//aem-headless-client.js`, que chama o ponto de acesso AEM GraphQL e retorna os dados de aventura.

Cada função, por sua vez, invoca o `aemHeadlessClient.runPersistedQuery(...)`, executando a consulta persistente do GraphQL.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### Páginas

O aplicativo Next.js usa duas páginas para apresentar os dados de aventura.

+ `src/pages/index.js`

  Usa getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) do [Next.js para chamar `getAllAdventures()` e exibe cada aventura como um cartão.

  O uso do `getServerSiteProps()` permite a Renderização no Servidor desta página Next.js.

+ `src/pages/adventures/[...slug].js`

  Uma [Rota Dinâmica Next.js](https://nextjs.org/docs/routing/dynamic-routes) que exibe os detalhes de uma única aventura. Essa rota dinâmica busca previamente os dados de cada aventura usando getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) do [Next.js por meio de uma chamada para `getAdventureBySlug(slug, queryVariables)` usando o parâmetro `slug` transmitido por meio da seleção de aventura na página `adventures/index.js` e `queryVariables` para controlar o formato, a largura e a qualidade da imagem.

  A rota dinâmica pode obter previamente os detalhes de todas as aventuras usando getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) do [Next.js e preenchendo todas as permutas de rotas possíveis com base na lista completa de aventuras retornadas pela consulta do GraphQL `getAdventurePaths()`

  O uso de `getStaticPaths()` e `getStaticProps(..)` permitiu a Geração de Site Estático dessas páginas Next.js.

## Configuração de implantação

Os aplicativos Next.js, especialmente no contexto de Renderização do lado do servidor (SSR) e Geração do lado do servidor (SSG), não exigem configurações de segurança avançadas, como o Compartilhamento de recursos entre origens (CORS).

No entanto, se o Next.js fizer solicitações HTTP para o AEM a partir do contexto do cliente, configurações de segurança no AEM podem ser necessárias. Revise o [tutorial de implantação de aplicativo de página única AEM Headless](../deployment/spa.md) para obter mais detalhes.
