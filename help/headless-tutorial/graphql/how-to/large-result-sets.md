---
title: Como trabalhar com grandes conjuntos de resultados no AEM Headless
description: Saiba como trabalhar com conjuntos de resultados grandes com AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 308
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---

# Grandes conjuntos de resultados no AEM Headless

As consultas do AEM Headless no GraphQL podem retornar grandes resultados. Este artigo descreve como trabalhar com grandes resultados no AEM Headless para garantir o melhor desempenho para seu aplicativo.

AEM Headless suporta um [deslocamento/limite](#list-query) e [paginação baseada em cursor](#paginated-query) consultas a subconjuntos menores de um conjunto de resultados maior. Várias solicitações podem ser feitas para coletar quantos resultados forem necessários.

Os exemplos abaixo usam pequenos subconjuntos de resultados (quatro registros por solicitação) para demonstrar as técnicas. Em um aplicativo real, você usaria um número maior de registros por solicitação para melhorar o desempenho. 50 registros por solicitação é uma boa linha de base.

## Modelo de fragmentos do conteúdo

A paginação e a classificação podem ser usadas em qualquer modelo de fragmento de conteúdo.

## Consultas persistentes do GraphQL

Ao trabalhar com grandes conjuntos de dados, a paginação baseada em cursor e deslocamento e limite pode ser usada para recuperar um subconjunto específico dos dados. No entanto, há algumas diferenças entre as duas técnicas que podem tornar uma mais apropriada do que a outra em determinadas situações.

### Deslocamento/limite

Listar consultas, usando `limit` e `offset` fornecer uma abordagem simples que especifique o ponto de partida (`offset`) e o número de registros a serem recuperados (`limit`). Essa abordagem permite que um subconjunto de resultados seja selecionado em qualquer lugar no conjunto de resultados completo, como pular para uma página específica de resultados. Embora seja fácil de implementar, pode ser lento e ineficiente ao lidar com resultados grandes, pois a recuperação de muitos registros requer a verificação de todos os registros anteriores. Essa abordagem também pode levar a problemas de desempenho quando o valor de deslocamento é alto, pois pode exigir a recuperação e o descarte de muitos resultados.

#### consulta do GraphQL

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### Variáveis de consulta

```json
{
  "offset": 1,
  "limit": 4
}
```

#### Resposta do GraphQL

A resposta JSON resultante contém a 2ª, 3ª, 4ª e 5ª Aventuras mais caras. As duas primeiras aventuras nos resultados têm o mesmo preço (`4500` assim, o [consulta de lista](#list-queries) especifica que aventuras com o mesmo preço são classificadas por título em ordem crescente.)

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### Consulta paginada

A paginação baseada em cursor, disponível em consultas Paginadas, envolve o uso de um cursor (uma referência a um registro específico) para recuperar o próximo conjunto de resultados. Essa abordagem é mais eficiente, pois evita a necessidade de examinar todos os registros anteriores para recuperar o subconjunto de dados necessário. As consultas paginadas são excelentes para iterar por meio de grandes conjuntos de resultados desde o início, até algum ponto no meio ou até o fim. Listar consultas, usando `limit` e `offset` fornecer uma abordagem simples que especifique o ponto de partida (`offset`) e o número de registros a serem recuperados (`limit`). Essa abordagem permite que um subconjunto de resultados seja selecionado em qualquer lugar no conjunto de resultados completo, como pular para uma página específica de resultados. Embora seja fácil de implementar, pode ser lento e ineficiente ao lidar com resultados grandes, pois a recuperação de muitos registros requer a verificação de todos os registros anteriores. Essa abordagem também pode levar a problemas de desempenho quando o valor de deslocamento é alto, pois pode exigir a recuperação e o descarte de muitos resultados.

#### consulta do GraphQL

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### Variáveis de consulta

```json
{
  "first": 3
}
```

#### Resposta do GraphQL

A resposta JSON resultante contém a 2ª, 3ª, 4ª e 5ª Aventuras mais caras. As duas primeiras aventuras nos resultados têm o mesmo preço (`4500` assim, o [consulta de lista](#list-queries) especifica que aventuras com o mesmo preço são classificadas por título em ordem crescente.)

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### Próximo conjunto de resultados paginados

O próximo conjunto de resultados pode ser obtido usando o `after` e o parâmetro `endCursor` valor da consulta anterior. Se não houver mais resultados a serem obtidos, `hasNextPage` é `false`.

##### Variáveis de consulta

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## Exemplos do React

Veja a seguir exemplos do React que demonstram como usar [deslocamento e limite](#offset-and-limit) e [paginação baseada em cursor](#cursor-based-pagination) abordagens. Normalmente, o número de resultados por solicitação é maior. No entanto, para os fins desses exemplos, o limite é definido como 5.

### Exemplo de deslocamento e limite

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

Usando deslocamento e limite, subconjuntos de resultados podem ser facilmente recuperados e exibidos.

#### gancho useEffect

A variável `useEffect` hook invoca uma consulta persistente (`adventures-by-offset-and-limit`) que recupera uma lista de Aventuras. A consulta usa o `offset` e `limit` parâmetros para especificar o ponto inicial e o número de resultados a serem recuperados. A variável `useEffect` o gancho é chamado quando a variável `page` alterações de valor.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### Componente

O componente usa o `useOffsetLimitAdventures` gancho para recuperar uma lista de Aventuras. A variável `page` O valor de é incrementado e diminuído para buscar o conjunto de resultados seguinte e anterior. A variável `hasMore` é usado para determinar se o botão next page deve ser ativado.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### Exemplo paginado

![Exemplo paginado](./assets/large-results/paginated-example.png)

_Cada caixa vermelha representa uma consulta HTTP GraphQL paginada distinta._

Usando a paginação baseada em cursor, grandes conjuntos de resultados podem ser facilmente recuperados e exibidos, coletando os resultados de forma incremental e concatenando-os aos resultados existentes.


#### Gancho UseEffect

A variável `useEffect` hook invoca uma consulta persistente (`adventures-by-paginated`) que recupera uma lista de Aventuras. A consulta usa o `first` e `after` parâmetros para especificar o número de resultados a serem recuperados e o cursor do qual iniciar. `fetchData` O realiza um loop contínuo, coletando o próximo conjunto de resultados paginados, até que não haja mais resultados a serem obtidos.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### Componente

O componente usa o `usePaginatedAdventures` gancho para recuperar uma lista de Aventuras. A variável `queryCount` value é utilizado para exibir o número de solicitações HTTP feitas para recuperar a lista de Aventuras.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
