---
title: Consultar AEM usando GraphQL de um aplicativo externo - Introdução ao AEM Headless - GraphQL
description: Introdução à Adobe Experience Manager (AEM) e GraphQL. Explore AEM APIs GraphQL como um exemplo do aplicativo WKND GraphQL React. Saiba como esse aplicativo externo faz chamadas GraphQL para AEM potencializar sua experiência. Saiba como executar o tratamento básico de erros.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1382'
ht-degree: 0%

---

# Consultar AEM usando GraphQL de um aplicativo externo

Neste capítulo, exploramos como AEM APIs GraphQL podem ser usadas para impulsionar a experiência em um aplicativo externo.

Este tutorial usa um aplicativo React simples para consultar e exibir conteúdo de Aventura exposto por APIs GraphQL AEM. O uso do React não é, em grande medida, importante e a aplicação externa de consumo poderia ser escrita em qualquer estrutura para qualquer plataforma.

## Pré-requisitos

Este é um tutorial com várias partes e presume-se que as etapas descritas nas partes anteriores foram concluídas.

_As capturas de tela do IDE neste capítulo vêm de [Código do Visual Studio](https://code.visualstudio.com/)_

Opcionalmente, instale uma extensão do navegador como [Inspetor de rede GraphQL](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) para exibir mais detalhes sobre uma consulta GraphQL.

## Objetivos

Neste capítulo, aprenderemos a:

* Comece e entenda a funcionalidade do aplicativo React de amostra
* Explore como as chamadas são feitas do aplicativo externo para AEM pontos finais GraphQL
* Definir uma consulta GraphQL para filtrar uma lista de Fragmentos de conteúdo de aventuras por atividade
* Atualize o React app para fornecer controles para filtrar via GraphQL, a lista de aventuras por atividade

## Inicie o aplicativo React

Como este capítulo se concentra no desenvolvimento de um cliente para consumir Fragmentos de conteúdo em GraphQL, a amostra [O código fonte do aplicativo WKND GraphQL React deve ser baixado e configurado](../quick-setup/local-sdk.md) no computador local.

A inicialização do aplicativo React é descrita com mais detalhes na seção [Configuração rápida](../quick-setup/local-sdk.md) capítulo, no entanto, as instruções resumidas podem ser seguidas:

1. Caso ainda não o tenha feito, clone o aplicativo WKND GraphQL de amostra do [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra o aplicativo WKND GraphQL React no IDE

   ![Reagir aplicativo no VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Na linha de comando, navegue até o `react-app` pasta
1. Inicie o aplicativo WKND GraphQL React executando o seguinte comando da raiz do projeto (o `react-app` folder)

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Revise o aplicativo em [http://localhost:3000/](http://localhost:3000/). O aplicativo React de amostra tem duas partes principais:

   * A experiência residencial atua como um índice de Aventuras WKND, ao consultar __Aventura__ Fragmentos de conteúdo no AEM usando GraphQL. Neste capítulo, modificaremos essa visualização para suportar a filtragem de aventuras por atividade.

      ![Aplicativo WKND GraphQL React - Experiência inicial](./assets/graphql-and-external-app/react-home-view.png)

   * A experiência de detalhes da aventura usa GraphQL para consultar o __Aventura__ Fragmento do conteúdo e exibe mais pontos de dados.

      ![Aplicativo de reação GraphQL WKND - Experiência detalhada](./assets/graphql-and-external-app/react-details-view.png)

1. Use as ferramentas de desenvolvimento do navegador e uma extensão do navegador como [Inspetor de rede GraphQL](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) para inspecionar as consultas GraphQL enviadas ao AEM e suas respostas JSON. Essa abordagem pode ser usada para monitorar solicitações e respostas GraphQL para garantir que elas sejam formuladas corretamente e suas respostas estejam conforme o esperado.

   ![Consulta bruta para adventureList](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *Consulta GraphQL enviada ao AEM do aplicativo React*

   ![Resposta de JSON GraphQL](assets/graphql-and-external-app/graphql-json-response.png)

   *Resposta JSON do AEM para o aplicativo React*

   As consultas e a resposta devem corresponder ao que foi visto no GraphiQL IDE.

   >[!NOTE]
   >
   > Durante o desenvolvimento, o aplicativo React é configurado para proxy de solicitações HTTP por meio do servidor de desenvolvimento do webpack para AEM. O aplicativo React está fazendo solicitações para  `http://localhost:3000` que os envia por proxy para o serviço AEM Author em execução em `http://localhost:4502`. Revise o arquivo `src/setupProxy.js` e `env.development` para obter detalhes.
   >
   > Em cenários que não são de desenvolvimento, o aplicativo React seria configurado diretamente para fazer solicitações ao AEM.

## Explore o código GraphQL do aplicativo

1. No IDE, abra o arquivo `src/api/useGraphQL.js`.

   Isso é uma [Gancho do efeito de reação](https://reactjs.org/docs/hooks-overview.html#effect-hook) que escuta alterações no `query`e, quando alterado, faz uma solicitação HTTP POST para o ponto final GraphQL da AEM e retorna a resposta JSON para o aplicativo.

   Sempre que o aplicativo React precisar fazer uma consulta GraphQL, ele chamará essa função personalizada `useGraphQL(query)` gancho, passando pelo GraphQL para enviar ao AEM.

   Este Gancho usa o `fetch` para fazer a solicitação HTTP POST GraphQL, mas outros módulos como o [Cliente Apollo GraphQL](https://www.apollographql.com/docs/react/) pode ser usada de forma semelhante.

1. Abrir `src/components/Adventures.js` no IDE, que é responsável pela listagem de aventuras da exibição inicial, e revise a invocação do `useGraphQL` gancho.

   Esse código define o padrão `query` para ser `allAdventuresQuery` conforme definido abaixo neste arquivo.

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   ... e a qualquer momento a `query` , a variável `useGraphQL` o gancho é chamado, que por sua vez executa a consulta GraphQL em relação ao AEM, retornando o JSON para o `data` , que é usada para renderizar a lista de aventuras.

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   O `allAdventuresQuery` é uma consulta GraphQL constante definida no arquivo, que consulta todos os Fragmentos de Conteúdo de Aventura, sem filtragem, e retorna somente o ponto de dados precisa renderizar a visualização inicial.

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

1. Abrir `src/components/AdventureDetail.js`, o componente React responsável pela exibição da experiência de detalhes da aventura. Essa exibição solicita um Fragmento do conteúdo específico, usando seu caminho JCR como sua id exclusiva e renderiza os detalhes fornecidos.

   Da mesma forma que `Adventures.js`, o `useGraphQL` O React Hook é reutilizado para executar essa consulta GraphQL em relação ao AEM.

   O caminho do Fragmento de conteúdo é coletado do componente `props` A parte superior pode ser usada para especificar o Fragmento de conteúdo para o qual consultar.

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ... e a consulta parametrizada GraphQL é construída usando o `adventureDetailQuery(..)` e passadas para `useGraphQL(query)` que executa a consulta GraphQL em relação ao AEM e retorna os resultados para o `data` variável.

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   O `adventureDetailQuery(..)` simplesmente envolve uma consulta GraphQL de filtragem, que usa AEM `<modelName>ByPath` para consultar um único Fragmento de conteúdo identificado por seu caminho JCR e retorna todos os pontos de dados especificados necessários para renderizar os detalhes da aventura.

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

## Criar uma consulta GraphQL parametrizada

Em seguida, vamos modificar o aplicativo React para realizar consultas GraphQL parametrizadas, que restringem a visualização inicial pela atividade das aventuras.

1. No IDE, abra o arquivo : `src/components/Adventures.js`. Esse arquivo representa o componente de aventuras da experiência inicial, que consulta e exibe os cartões de Aventuras.
1. Inspect a função `filterQuery(activity)`, que não é utilizado, mas foi preparado para formular uma consulta GraphQL que filtre aventuras por `activity`.

   Observe que o parâmetro `activity` é inserido na consulta GraphQL como parte de um `filter` no `adventureActivity` , exigindo que o valor desse campo corresponda ao valor do parâmetro.

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

1. Atualize o componente React Adventures `return` instrução para adicionar botões que chamam o novo parametrizado `filterQuery(activity)` para fornecer as aventuras a serem listadas.

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

1. Salve as alterações e recarregue o aplicativo React no navegador da Web. Os três novos botões aparecem na parte superior e o clique neles faz uma nova consulta automaticamente AEM Fragmentos de conteúdo do Adventure com a atividade correspondente.

   ![Filtrar Aventuras por Atividade](./assets/graphql-and-external-app/filter-by-activity.png)

1. Tente adicionar mais botões de filtragem para as atividades: `Rock Climbing`, `Cycling` e `Skiing`

## Gerenciar erros de GraphQL

GraphQL é altamente digitado e, portanto, pode retornar mensagens de erro úteis se a consulta for inválida. Em seguida, vamos simular uma consulta incorreta para ver a mensagem de erro retornada.

1. Reabra o arquivo `src/api/useGraphQL.js`. Inspect o seguinte trecho para ver o tratamento de erros:

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

   A resposta é inspecionada para ver se inclui uma `errors` objeto. O `errors` será enviado por AEM se houver problemas com a consulta GraphQL, como um campo indefinido com base no esquema. Se não houver `errors` o objeto `data` é definido e retornado.

   O `window.fetch` inclui um `.catch` instrução para *captura* qualquer erro comum, como uma solicitação HTTP inválida ou se a conexão com o servidor não puder ser feita.

1. Abra o arquivo `src/components/Adventures.js`.
1. Modifique o `allAdventuresQuery` para incluir uma propriedade inválida `adventurePetPolicy`:

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

   Sabemos que `adventurePetPolicy` não faz parte do modelo da Aventura, portanto, deve disparar um erro.

1. Salve as alterações e retorne ao navegador. Você deve ver uma mensagem de erro como a seguinte:

   ![Erro de Propriedade Inválido](assets/graphql-and-external-app/invalidProperty.png)

   A API GraphQL detecta que `adventurePetPolicy` é indefinido no `AdventureModel` e retorna uma mensagem de erro apropriada.

1. Inspect a resposta de AEM usando as ferramentas de desenvolvedor do navegador para visualizar o `errors` Objeto JSON:

   ![Erros de objeto JSON](assets/graphql-and-external-app/error-json-response.png)

   O `errors` é detalhado e inclui informações sobre o local da consulta malformada e a classificação do erro.

1. Retornar para `Adventures.js` e reverter a alteração da consulta, para retornar o aplicativo ao seu estado adequado.

## Parabéns!{#congratulations}

Parabéns! Você explorou com sucesso o código do aplicativo WKND GraphQL React da amostra e o atualizou para o uso de consultas GraphQL com parâmetros filtrados e parametrizadas para listar aventuras por atividade! Você também tem a oportunidade de explorar algumas práticas básicas de tratamento de erros.

## Próximas etapas {#next-steps}

No próximo capítulo, [Modelagem de dados avançada com referências de fragmento](./fragment-references.md) você aprenderá a usar o recurso Referência de fragmento para criar uma relação entre dois Fragmentos de conteúdo diferentes. Você também aprenderá a modificar uma consulta GraphQL para incluir um campo de um modelo referenciado.
