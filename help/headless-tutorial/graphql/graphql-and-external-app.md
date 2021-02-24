---
title: Query AEM usando o GraphQL de um aplicativo externo - Introdução ao AEM sem cabeçalho - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e ao GraphQL. Explore AEM APIs GraphQL como um aplicativo de exemplo do WKND GraphQL React. Saiba como esse aplicativo externo faz chamadas ao GraphQL para AEM potencializar sua experiência. Saiba como executar a manipulação básica de erros.
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
translation-type: tm+mt
source-git-commit: ce4a35f763862c6d6a42795fd5e79d9c59ff645a
workflow-type: tm+mt
source-wordcount: '1397'
ht-degree: 0%

---


# Query AEM usando o GraphQL de um aplicativo externo

Neste capítulo, exploramos como AEM APIs GraphQL podem ser usadas para direcionar a experiência em um aplicativo externo.

Este tutorial usa um aplicativo React simples para query e exibe conteúdo Adventure exposto por AEM APIs GraphQL. O uso do React não é importante em grande parte, e o aplicativo externo que consome poderia ser escrito em qualquer estrutura para qualquer plataforma.

## Pré-requisitos

Este é um tutorial de várias partes e presume-se que as etapas descritas nas partes anteriores foram concluídas.

_Capturas de tela IDE neste capítulo vêm do Código do  [Visual Studio](https://code.visualstudio.com/)_

Como opção, instale uma extensão do navegador como [GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm) para poder visualização mais detalhes sobre um query GraphQL.

## Objetivos

Neste capítulo, aprenderemos a:

* Start e compreensão da funcionalidade do aplicativo React de amostra
* Explore como as chamadas são feitas do aplicativo externo para AEM pontos finais do GraphQL
* Definir um query GraphQL para filtrar uma lista de fragmentos de conteúdo de aventuras por atividade
* Atualize o aplicativo React para fornecer controles para filtrar por GraphQL, a lista de aventuras por atividade

## Start do aplicativo React

Como este capítulo se concentra no desenvolvimento de um cliente para consumir Fragmentos de Conteúdo em GraphQL, a amostra [Código de origem do aplicativo WKND GraphQL React deve ser baixada e configurada](./setup.md#react-app) no computador local, e o [AEM SDK está sendo executado como o serviço de autor](./setup.md#aem-sdk) com a amostra [Site WKND instalado](./setup.md#wknd-site).

A inicialização do aplicativo React é descrita com mais detalhes no capítulo [Quick Setup](./setup.md), no entanto, as instruções abreviadas podem ser seguidas:

1. Caso ainda não o tenha feito, clone o aplicativo WKND GraphQL React de amostra de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra o aplicativo WKND GraphQL React no IDE

   ![Reagir aplicativo no VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Na linha de comando, navegue até a pasta `react-app`
1. Start o aplicativo WKND GraphQL React, executando o seguinte comando da raiz do projeto (a pasta `react-app`)

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Revise o aplicativo em [http://localhost:3000/](http://localhost:3000/). A amostra do aplicativo React tem duas partes principais:

   * A experiência residencial atua como um índice de Aventuras WKND, consultando __Aventura__ Fragmentos de Conteúdo em AEM usando o GraphQL. Neste capítulo, modificaremos esta visualização para suportar a filtragem de aventuras por atividade.

      ![Aplicativo WKND GraphQL React - Experiência inicial](./assets/graphql-and-external-app/react-home-view.png)

   * A experiência de detalhes da aventura usa o GraphQL para query do __Fragmento de conteúdo do Adventure__ específico e exibe mais pontos de dados.

      ![Aplicativo WKND GraphQL React - Experiência detalhada](./assets/graphql-and-external-app/react-details-view.png)

1. Use as ferramentas de desenvolvimento do navegador e uma extensão do navegador, como [GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm), para inspecionar os query GraphQL enviados para AEM e suas respostas JSON. Essa abordagem pode ser usada para monitorar solicitações e respostas do GraphQL para garantir que elas sejam formuladas corretamente e que suas respostas sejam as esperadas.

   ![Query bruto para adventureList](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *Query GraphQL enviado para AEM do aplicativo React*

   ![Resposta do GraphQL JSON](assets/graphql-and-external-app/graphql-json-response.png)

   *Resposta JSON do AEM para o aplicativo React*

   Os query e a resposta devem corresponder ao que foi visto no GraphiQL IDE.

   >[!NOTE]
   >
   > Durante o desenvolvimento, o aplicativo React é configurado para proxy de solicitações HTTP por meio do servidor de desenvolvimento do webpack para AEM. O aplicativo React está fazendo solicitações para `http://localhost:3000`, que as proxy para o serviço de autor de AEM em execução em `http://localhost:4502`. Consulte os arquivos `src/setupProxy.js` e `env.development` para obter detalhes.
   >
   > Em cenários de não desenvolvimento, o aplicativo React seria configurado diretamente para fazer solicitações a AEM.

## Explore o código GraphQL do aplicativo

1. No IDE, abra o arquivo `src/api/useGraphQL.js`.

   Este é um [React Effect Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook) que escuta as alterações no `query` do aplicativo, e, ao alterar, faz uma solicitação HTTP POST para o ponto final AEM GraphQL e retorna a resposta JSON para o aplicativo.

   Sempre que o aplicativo React precisar criar um query GraphQL, ele chamará esse gancho `useGraphQL(query)` personalizado, transmitindo o GraphQL para enviar ao AEM.

   Este gancho usa o módulo simples `fetch` para fazer a solicitação HTTP POST GraphQL, mas outros módulos como o [Apollo GraphQL client](https://www.apollographql.com/docs/react/) podem ser usados de forma semelhante.

1. Abra `src/components/Adventures.js` no IDE, que é responsável pela listagem de aventuras de visualizações residenciais, e reveja a invocação do gancho `useGraphQL`.

   Esse código define o padrão `query` como `allAdventuresQuery` conforme definido abaixo neste arquivo.

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   ... e sempre que a variável `query` for alterada, o gancho `useGraphQL` será chamado, que por sua vez executa o query GraphQL contra AEM, retornando o JSON para a variável `data`, que é então usada para renderizar a lista de aventuras.

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   O `allAdventuresQuery` é um query GraphQL constante definido no arquivo, que query todos os Fragmentos de Conteúdo da Aventura, sem qualquer filtro, e retorna somente os pontos de dados precisam renderizar a visualização inicial.

   ```javascript
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
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
   `;
   ```

1. Abra `src/components/AdventureDetail.js`, o componente React responsável por exibir a experiência de detalhes da aventura. Esta visualização solicita um fragmento de conteúdo específico, usando seu caminho JCR como sua id exclusiva e renderiza os detalhes fornecidos.

   Da mesma forma que `Adventures.js`, o gancho de reação `useGraphQL` personalizado é reutilizado para executar o query GraphQL em relação ao AEM.

   O caminho do Fragmento de conteúdo é coletado da parte superior `props` do componente a ser usada para especificar o Fragmento de conteúdo para o query.

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ... e o query parametrizado GraphQL é construído usando a função `adventureDetailQuery(..)` e passado para `useGraphQL(query)`, que executa o query GraphQL em relação ao AEM e retorna os resultados para a variável `data`.

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   A função `adventureDetailQuery(..)` simplesmente envolve um query GraphQL de filtragem, que usa a sintaxe AEM `<modelName>ByPath` para query de um único Fragmento de conteúdo identificado pelo caminho JCR, e retorna todos os pontos de dados especificados necessários para renderizar os detalhes da aventura.

   ```javascript
   function adventureDetailQuery(_path) {
   return `{
       adventureByPath (_path: "${_path}") {
         item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
         }
       }
   }
   `;
   }
   ```

## Criar um query GraphQL com parâmetros

Em seguida, vamos modificar o aplicativo React para executar query GraphQL com parâmetros de filtragem que restringem a visualização inicial pela atividade das aventuras.

1. No IDE, abra o arquivo: `src/components/Adventures.js`. Esse arquivo representa o componente de aventuras da experiência residencial, que query e exibe os cartões de Aventuras.
1. Inspect a função `filterQuery(activity)`, que não é usada, mas que foi preparada para formular um query GraphQL que filtros aventuras por `activity`.

   Observe que o parâmetro `activity` é inserido no query GraphQL como parte de um `filter` no campo `adventureActivity`, exigindo que o valor desse campo corresponda ao valor do parâmetro.

   ```javascript
   function filterQuery(activity) {
       return `
           {
           adventures (filter: {
               adventureActivity: {
               _expressions: [
                   {
                   value: "${activity}"
                   }
                 ]
               }
           }){
               items {
               _path
               adventureTitle
               adventurePrice
               adventureTripLength
               adventurePrimaryImage {
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
       `;
   }
   ```

1. Atualize a instrução `return` do componente React Adventures para adicionar botões que invocam o novo `filterQuery(activity)` parametrizado para fornecer as aventuras à lista.

   ```javascript
   function Adventures() {
       ...
       return (
           <div className="adventures">
   
           {/* Add these three new buttons that set the GraphQL query accordingly */}
   
           {/* The first button uses the default `allAdventuresQuery` */}
           <button onClick={() => setQuery(allAdventuresQuery)}>All</button>
   
           {/* The 2nd and 3rd button use the `filterQuery(..)` to filter by activity */}
           <button onClick={() => setQuery(filterQuery('Camping'))}>Camping</button>
           <button onClick={() => setQuery(filterQuery('Surfing'))}>Surfing</button>
   
           <ul className="adventure-items">
           ...
       )
   }
   ```

1. Salve as alterações e recarregue o aplicativo React no navegador da Web. Os três novos botões são exibidos na parte superior e, ao clicar neles, os query são automaticamente refeitos AEM para Fragmentos de conteúdo da Adobe com a atividade correspondente.

   ![Filtrar Aventuras por Atividade](./assets/graphql-and-external-app/filter-by-activity.png)

1. Tente adicionar mais botões de filtragem para as atividades: `Rock Climbing`, `Cycling` e `Skiing`

## Lidar com erros de GraphQL

O GraphQL é fortemente digitado e, portanto, pode retornar mensagens de erro úteis se o query for inválido. Em seguida, vamos simular um query incorreto para ver a mensagem de erro retornada.

1. Abra novamente o arquivo `src/api/useGraphQL.js`. Inspect o seguinte snippet para ver a manipulação de erros:

   ```javascript
   //useGraphQL.js
   .then(({data, errors}) => {
           //If there are errors in the response set the error message
           if(errors) {
               setErrors(mapErrors(errors));
           }
           //Otherwise if data in the response set the data as the results
           if(data) {
               setData(data);
           }
       })
       .catch((error) => {
           setErrors(error);
       });
   ```

   A resposta é inspecionada para verificar se inclui um objeto `errors`. O objeto `errors` será enviado por AEM se houver problemas com o query GraphQL, como um campo indefinido com base no schema. Se não houver nenhum objeto `errors`, `data` será definido e retornado.

   O `window.fetch` inclui uma instrução `.catch` para *catch* quaisquer erros comuns, como uma solicitação HTTP inválida ou se a conexão com o servidor não puder ser feita.

1. Abra o arquivo `src/components/Adventures.js`.
1. Modifique `allAdventuresQuery` para incluir uma propriedade inválida `adventurePetPolicy`:

   ```javascript
   /**
    * Query for all Adventures
    * adventurePetPolicy has been added beneath items
   */
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           adventurePetPolicy
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
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
   `;
   ```

   Sabemos que `adventurePetPolicy` não faz parte do modelo da Adventure, portanto isso deve disparar um erro.

1. Salve as alterações e volte ao navegador. Você deve ver uma mensagem de erro como a seguinte:

   ![Erro de propriedade inválido](assets/graphql-and-external-app/invalidProperty.png)

   A API GraphQL detecta que `adventurePetPolicy` está indefinido em `AdventureModel` e retorna uma mensagem de erro apropriada.

1. Inspect a resposta de AEM usando as ferramentas de desenvolvedor do navegador para ver o objeto `errors` JSON:

   ![Erro de objeto JSON](assets/graphql-and-external-app/error-json-response.png)

   O objeto `errors` é detalhado e inclui informações sobre a localização do query malformado e a classificação do erro.

1. Retorne para `Adventures.js` e reverta a alteração de query para retornar o aplicativo ao seu estado adequado.

## Parabéns!{#congratulations}

Parabéns! Você explorou com êxito o código da amostra do aplicativo WKND GraphQL React e o atualizou para usar query GraphQL com parâmetros de filtragem e parametrizados para aventuras de lista por atividade! Você também tem a chance de explorar algumas manipulações básicas de erros.

## Próximas etapas {#next-steps}

No próximo capítulo, [Modelagem de dados avançada com Referências de fragmento](./fragment-references.md) você aprenderá como usar o recurso Referência de fragmento para criar uma relação entre dois Fragmentos de conteúdo diferentes. Você também aprenderá a modificar um query GraphQL para incluir um campo de um modelo referenciado.
