---
title: Next.js - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Next.js demonstra como consultar conteúdo usando APIs GraphQL AEM usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 2%

---

# Aplicativo Next.js

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Next.js demonstra como consultar conteúdo usando APIs GraphQL AEM usando consultas persistentes. O Cliente Sem Cabeçalho do AEM para JavaScript é usado para executar as consultas persistentes de GraphQL que alimentam o aplicativo.

![Aplicativo Next.js com AEM headless](./assets/next-js/next-js.png)

Visualize o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## Requisitos AEM

O aplicativo Next.js funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem [WKND Compartilhado v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [Site WKND v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)ou o [Complemento de demonstração de referência](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) para ser instalado no ambiente as a Cloud Service AEM.

Este exemplo de aplicativo Next.js foi projetado para se conectar a __Publicação do AEM__ serviço.

### Requisitos do autor do AEM

O Next.js foi projetado para se conectar a __Publicação do AEM__ e acessar conteúdo desprotegido. O Next.js pode ser configurado para se conectar ao AEM Author por meio do `.env` as propriedades descritas abaixo. As imagens veiculadas pelo autor do AEM exigem autenticação e, portanto, o usuário que acessar o aplicativo Next.js também deve ser conectado ao autor do AEM.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd-graphql/next-js/.env.local` arquivo e conjunto `NEXT_PUBLIC_AEM_HOST` ao serviço AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Se estiver se conectando ao serviço Autor do AEM, a autenticação deve ser fornecida, pois o serviço Autor do AEM é seguro por padrão.

   Para usar um conjunto de contas de AEM local `AEM_AUTH_METHOD=basic` e forneça o nome de usuário e a senha no `AEM_AUTH_USER` e `AEM_AUTH_PASSWORD` propriedades.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Para usar um [Token de desenvolvimento local AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` e fornecer o valor do token de desenvolvimento completo na `AEM_AUTH_DEV_TOKEN` propriedade.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Edite o `aem-guides-wknd-graphql/next-js/.env.local` arquivo e validação  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` é definida como o ponto de extremidade GraphQL do AEM apropriado.

   Ao usar [WKND Compartilhado](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou [Site WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), use o `wknd-shared` Ponto de extremidade da API GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   Ao usar [Complemento de demonstração de referência](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html), use o `aem-demo-assets` Ponto de extremidade da API GraphQL.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
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

Abaixo está um resumo de como o aplicativo Next.js é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

Seguindo AEM práticas recomendadas headless, o aplicativo Next.js usa consultas persistentes AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

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

Cada consulta persistente tem uma função correspondente em `src/lib//aem-headless-client.js`, que chama o ponto final GraphQL da AEM e retorna os dados de aventura.

Cada função, por sua vez, chama a função `aemHeadlessClient.runPersistedQuery(...)`, executando a consulta GraphQL mantida.

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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### Páginas

O aplicativo Next.js usa duas páginas para apresentar os dados de aventura.

+ `src/pages/index.js`

   Usos [getServerSideProps() da próxima.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) para chamar `getAllAdventures()` e exibe cada aventura como um cartão.

   O uso de `getServerSiteProps()` permite a renderização do lado do servidor desta página Next.js.

+ `src/pages/adventures/[...slug].js`

   A [Rota Dinâmica Next.js](https://nextjs.org/docs/routing/dynamic-routes) que exibe os detalhes de uma única aventura. Essa rota dinâmica busca previamente os dados de cada aventura usando [getStaticProps() da próxima.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) por meio de uma chamada para `getAdventureBySlug(..)` usando o `slug` param transmitido por meio da seleção de aventura no `adventures/index.js` página.

   A rota dinâmica pode pré-buscar os detalhes de todas as aventuras usando [getStaticPaths() da próxima.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) e preencher todas as permutas de rota possíveis com base na lista completa de aventuras retornadas pela consulta GraphQL  `getAdventurePaths()`

   O uso de `getStaticPaths()` e `getStaticProps(..)` permitiu a geração de site estático dessas páginas Next.js.

## Configuração de implantação

Os aplicativos Next.js, especialmente no contexto de renderização do lado do servidor (SSR) e geração do lado do servidor (SSG), não exigem configurações de segurança avançadas, como CORS (Cross-origin Resource Sharing).

No entanto, se o Next.js não fizer solicitações HTTP para AEM a partir do contexto do cliente, as configurações de segurança no AEM poderão ser necessárias. Revise o [AEM Tutorial de implantação de aplicativo de página única sem cabeçalho](../deployment/spa.md) para obter mais detalhes.

