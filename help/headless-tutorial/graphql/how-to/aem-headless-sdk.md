---
title: Uso do SDK sem cabeçalho do AEM
description: Saiba como fazer consultas GraphQL usando o SDK sem cabeçalho do AEM.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 595d990b7d8ed3c801a085892fef38d780082a15
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 4%

---

# SDK sem cabeçalho AEM

O SDK sem cabeçalho AEM é um conjunto de bibliotecas que podem ser usadas pelos clientes para interagir rápida e facilmente com AEM APIs sem cabeçalho por HTTP.

O SDK sem cabeçalho do AEM está disponível para várias plataformas:

+ [AEM SDK sem cabeçalho para navegadores do lado do cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK sem cabeçalho AEM para server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK sem periféricos AEM para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas persistentes de GraphQL 

Consulta de AEM usando GraphQL usando consultas persistentes (em vez de [consultas GraphQL definidas pelo cliente](#graphl-queries)) permite que os desenvolvedores persistam em uma consulta (mas não em seus resultados) no AEM e, em seguida, solicitem que a consulta seja executada por nome. As consultas persistentes são semelhantes ao conceito de procedimentos armazenados em bancos de dados SQL.

As consultas persistentes são mais eficazes do que as consultas GraphQL definidas pelo cliente, pois as consultas persistentes são executadas usando HTTP GET, que pode ser armazenado em cache nos níveis CDN e AEM Dispatcher. Consultas persistentes também estão em vigor, definem uma API e dissociam a necessidade do desenvolvedor entender os detalhes de cada Modelo de fragmento de conteúdo.

### Exemplos de código{#persisted-graphql-queries-code-examples}

A seguir estão exemplos de código de como executar uma consulta persistente de GraphQL em relação ao AEM.

+++ Exemplo de JavaScript

Instale o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) executando o `npm install` da raiz do seu projeto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Este exemplo de código mostra como consultar AEM usando o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) módulo npm usando `async/await` sintaxe. O SDK sem cabeçalho AEM para JavaScript também é compatível [Sintaxe de promessa](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

Este código assume uma consulta persistente com o nome `wknd/adventureNames` O foi criado no AEM Author e publicado na AEM Publish.

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

Instale o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) executando o `npm install` da raiz do projeto React.

```
$ npm i @adobe/aem-headless-client-js
```

Este exemplo de código mostra como usar o [React useEffect(..) gancho](https://reactjs.org/docs/hooks-effect.html) para executar uma chamada assíncrona para AEM GraphQL.

Usando `useEffect` para fazer a chamada GraphQL assíncrona no React é útil porque:

1. Ele fornece wrapper síncrono para a chamada assíncrona para AEM.
1. Reduz a repetição desnecessária de AEM.

Este código assume uma consulta persistente com o nome `wknd-shared/adventure-by-slug` O foi criado no AEM Author e publicado na AEM Publish usando GraphiQL.

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

Chamar o Reato personalizado `useEffect` gancho de outro lugar em um componente React .

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

Novo `useEffect` os ganchos podem ser criados para cada consulta persistente que o aplicativo React usa.

+++

<p> </p>

## Consultas GraphQL

AEM suporta consultas GraphQL definidas pelo cliente, no entanto, AEM prática recomendada usar [consultas GraphQL persistentes](#persisted-graphql-queries).

