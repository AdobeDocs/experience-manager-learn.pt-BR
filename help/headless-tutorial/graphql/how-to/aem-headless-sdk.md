---
title: Uso do SDK AEM Headless
description: Saiba como fazer consultas ao GraphQL usando o AEM Headless SDK.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
duration: 200
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 6%

---

# SDK do AEM Headless

O SDK do AEM Headless é um conjunto de bibliotecas que podem ser usadas pelos clientes para interagir rápida e facilmente com APIs do AEM Headless por HTTP.

O SDK do AEM Headless está disponível para várias plataformas:

+ [SDK headless do AEM para navegadores do lado do cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK headless do AEM para lado do servidor/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK headless do AEM para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas persistentes de GraphQL

Consultar o AEM usando o GraphQL usando consultas persistentes (em vez de [consultas do GraphQL definidas pelo cliente](#graphl-queries)) permite que os desenvolvedores persistam uma consulta (mas não seus resultados) no AEM e, em seguida, solicita que a consulta seja executada pelo nome. As consultas persistentes são semelhantes ao conceito de procedimentos armazenados em bancos de dados SQL.

As consultas persistentes têm melhor desempenho do que as consultas GraphQL definidas pelo cliente, já que as consultas persistentes são executadas usando HTTP GET, que pode ser armazenado em cache nos níveis de CDN e AEM Dispatcher. As consultas persistentes também estão em vigor, definem uma API e dissociam a necessidade de o desenvolvedor entender os detalhes de cada modelo de fragmento de conteúdo.

### Exemplos de código{#persisted-graphql-queries-code-examples}

A seguir estão exemplos de código de como executar uma consulta persistente do GraphQL com AEM.

+++ Exemplo do JavaScript

Instale o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) executando o comando `npm install` da raiz do seu projeto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Este exemplo de código mostra como consultar AEM usando o módulo [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) npm com sintaxe `async/await`. O SDK AEM Headless para JavaScript também oferece suporte à [sintaxe de promessa](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Este código supõe que uma consulta persistente com o nome `wknd/adventureNames` foi criada no AEM Author e publicada no AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) exemplo

Instale o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) executando o comando `npm install` da raiz do seu projeto React.

```
$ npm i @adobe/aem-headless-client-js
```

Este exemplo de código mostra como usar o [React useEffect(..) conecte ](https://reactjs.org/docs/hooks-effect.html) para executar uma chamada assíncrona ao AEM GraphQL.

Usar o `useEffect` para fazer a chamada assíncrona do GraphQL no React é útil porque:

1. Ele fornece wrapper síncrono para a chamada assíncrona ao AEM.
1. Reduz o AEM requerendo desnecessariamente.

Este código supõe que uma consulta persistente com o nome `wknd-shared/adventure-by-slug` foi criada no AEM Author e publicada no AEM Publish usando GraphiQL.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

Invoque o gancho personalizado `useEffect` do React de outro lugar em um componente do React.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Novos ganchos `useEffect` podem ser criados para cada consulta persistente que o aplicativo React usa.

+++

<p> </p>

## consultas do GraphQL

O AEM oferece suporte a consultas GraphQL definidas pelo cliente, no entanto, é prática recomendada do AEM usar [consultas GraphQL persistentes](#persisted-graphql-queries).

## Webpack 5+

O SDK JS do AEM Headless tem dependências em `util` que não estão incluídas no Webpack 5+ por padrão. Se você estiver usando o Webpack 5+, e receber o seguinte erro:

```
Compiled with problems:
× ERROR in ./node_modules/@adobe/aio-lib-core-errors/src/AioCoreSDKErrorWrapper.js 12:13-28
Module not found: Error: Can't resolve 'util' in '/Users/me/Code/wknd-headless-examples/node_modules/@adobe/aio-lib-core-errors/src'

BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.

If you want to include a polyfill, you need to:
    - add a fallback 'resolve.fallback: { "util": require.resolve("util/") }'
    - install 'util'
If you don't want to include a polyfill, you can use an empty module like this:
    resolve.fallback: { "util": false }
```

Adicione o seguinte `devDependencies` ao seu arquivo `package.json`:

```json
  "devDependencies": {
    "buffer": "npm:buffer@^6.0.3",
    "crypto": "npm:crypto-browserify@^3.12.0",
    "http": "npm:stream-http@^3.2.0",
    "https": "npm:https-browserify@^1.0.0",
    "stream": "npm:stream-browserify@^3.0.0",
    "util": "npm:util@^0.12.5",
    "zlib": "npm:browserify-zlib@^0.2.0"
  },
```

Em seguida, execute `npm install` para instalar as dependências.
