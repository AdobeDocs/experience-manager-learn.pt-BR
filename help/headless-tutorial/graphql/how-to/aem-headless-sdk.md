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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# SDK sem cabeçalho AEM

O SDK sem cabeçalho AEM é um conjunto de bibliotecas que podem ser usadas pelos clientes para interagir rápida e facilmente com AEM APIs sem cabeçalho por HTTP.

O SDK sem cabeçalho do AEM está disponível para várias plataformas:

+ [AEM SDK sem cabeçalho para navegadores do lado do cliente (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [SDK sem cabeçalho AEM para server-side/Node.js (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [SDK sem periféricos AEM para Java™](https://github.com/adobe/aem-headless-client-java)

## Consultas GraphQL

Consulta de AEM usando GraphQL usando consultas (em vez de [consultas GraphQL persistentes](#persisted-graphql-queries)) permite que os desenvolvedores definam consultas no código, especificando exatamente qual conteúdo solicitar do AEM.

As consultas GraphQL tendem a ter menos desempenho do que as consultas persistentes, pois são executadas usando HTTP POST, que é menos cache nas camadas CDN e AEM Dispatcher.

### Exemplos de código{#graphql-queries-code-examples}

A seguir estão exemplos de código de como executar uma consulta GraphQL em relação ao AEM.

+++ Exemplo de JavaScript

Instale o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) executando o `npm install` da raiz do seu projeto Node.js.

```
$ npm i @adobe/aem-headless-client-js
```

Este exemplo de código mostra como consultar AEM usando o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) módulo npm usando `async/await` sintaxe. O SDK sem cabeçalho AEM para JavaScript também é compatível [Sintaxe de promessa](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
```

+++


+++ React useEffect(..) exemplo

Instale o [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) executando o `npm install` da raiz do projeto React.

```
$ npm i @adobe/aem-headless-client-js
```

Este exemplo de código mostra como usar o [React useEffect(..) gancho](https://reactjs.org/docs/hooks-effect.html) para executar uma chamada assíncrona para AEM GraphQL.

Usando `useEffect` para fazer a chamada GraphQL assíncrona no React é útil, pois ela:

1. Fornece wrapper síncrono para a chamada assíncrona para AEM.
1. Reduz a AEM de requerê-la desnecessariamente.

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

Importe e use o `useGraphQL` gancho no componente React para consultar AEM.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## Consultas persistentes de GraphQL 

Consulta de AEM usando GraphQL usando consultas persistentes (em vez de [consultas GraphQL regulares](#graphl-queries)) permite que os desenvolvedores persistam em uma consulta (mas não em seus resultados) no AEM e, em seguida, solicitem que a consulta seja executada por nome. As consultas persistentes são semelhantes ao conceito de procedimentos armazenados em bancos de dados SQL.

As consultas persistentes tendem a ter mais desempenho do que as consultas GraphQL regulares, já que as consultas persistentes são executadas usando HTTP GET, que é mais capaz de armazenar em cache nos níveis de CDN e AEM Dispatcher. Consultas persistentes também estão em vigor, definem uma API e dissociam a necessidade do desenvolvedor entender os detalhes de cada Modelo de fragmento de conteúdo.

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
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

Este código assume uma consulta persistente com o nome `wknd/adventureNames` O foi criado no AEM Author e publicado na AEM Publish.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

E chame esse código de outro lugar no código React.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
