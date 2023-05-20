---
title: Next.js - Exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo Next.js demonstra como consultar conteúdo usando APIs AEM GraphQL usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 1%

---

# Aplicativo Next.js

Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo Next.js demonstra como consultar conteúdo usando APIs AEM GraphQL usando consultas persistentes. O AEM Headless Client para JavaScript é usado para executar as consultas persistentes do GraphQL que alimentam o aplicativo.

![Aplicativo Next.js com AEM Headless](./assets/next-js/next-js.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo Next.js funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem [WKND compartilhado v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou [Site WKND v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) para ser instalado no ambiente as a Cloud Service AEM.

Este aplicativo Next.js de exemplo foi projetado para se conectar a __AEM Publish__ serviço.

### Requisitos do AEM Author

O Next.js foi projetado para se conectar a __AEM Publish__ e acessar conteúdo desprotegido. O Next.js pode ser configurado para se conectar ao AEM Author por meio do `.env` propriedades descritas abaixo. As imagens veiculadas pelo AEM Author exigem autenticação e, portanto, o usuário que acessar o aplicativo Next.js também deve estar conectado ao AEM Author.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd-graphql/next-js/.env.local` arquivo e definir `NEXT_PUBLIC_AEM_HOST` ao serviço AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Se você se conectar ao serviço do AEM Author, a autenticação deverá ser fornecida, já que o serviço do AEM Author é seguro por padrão.

   Para usar um conjunto de contas do AEM local `AEM_AUTH_METHOD=basic` e forneça o nome de usuário e a senha no campo `AEM_AUTH_USER` e `AEM_AUTH_PASSWORD` propriedades.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Para usar uma [Token de desenvolvimento local as a Cloud Service para AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` e fornecer o valor total do token dev na variável `AEM_AUTH_DEV_TOKEN` propriedade.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Edite o `aem-guides-wknd-graphql/next-js/.env.local` arquivo e validar  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` está definido para o endpoint AEM GraphQL apropriado.

   Ao usar [WKND compartilhado](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) ou [Site da WKND](https://github.com/adobe/aem-guides-wknd/releases/latest), use o `wknd-shared` Endpoint da API do GraphQL.

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

Abaixo está um resumo de como o aplicativo Next.js é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo Next.js usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

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

Cada consulta persistente tem uma função correspondente no `src/lib//aem-headless-client.js`, que chama o ponto de acesso do AEM GraphQL e retorna os dados de aventura.

Cada função, por sua vez, chama a variável `aemHeadlessClient.runPersistedQuery(...)`, executando a consulta persistente do GraphQL.

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

   Usos [getServerSideProps() da Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) para chamar `getAllAdventures()` e exibe cada aventura como um cartão.

   A utilização de `getServerSiteProps()` permite a renderização do lado do servidor desta página Next.js.

+ `src/pages/adventures/[...slug].js`

   A [Rota dinâmica do Next.js](https://nextjs.org/docs/routing/dynamic-routes) que mostra os detalhes de uma única aventura. Essa rota dinâmica busca previamente os dados de cada aventura usando [getStaticProps() da Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) por meio de uma chamada para `getAdventureBySlug(..)` usando o `slug` parâmetro passado pela seleção de aventura no `adventures/index.js` página.

   A rota dinâmica é capaz de obter previamente os detalhes de todas as aventuras usando [getStaticPaths() da Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) e preenchendo todas as permutações de rota possíveis com base na lista completa de aventuras retornadas pelo query do GraphQL  `getAdventurePaths()`

   A utilização de `getStaticPaths()` e `getStaticProps(..)` permitido a Geração de site estático dessas páginas Next.js.

## Configuração de implantação

Os aplicativos Next.js, especialmente no contexto de Renderização do lado do servidor (SSR) e Geração do lado do servidor (SSG), não exigem configurações de segurança avançadas, como o Compartilhamento de recursos entre origens (CORS).

No entanto, se o Next.js fizer solicitações HTTP para o AEM a partir do contexto do cliente, configurações de segurança no AEM podem ser necessárias. Revise o [Tutorial de implantação do aplicativo AEM headless de página única](../deployment/spa.md) para obter mais detalhes.
