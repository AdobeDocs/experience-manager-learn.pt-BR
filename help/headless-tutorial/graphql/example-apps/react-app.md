---
title: Aplicativo React - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). É fornecido um aplicativo React que demonstra como consultar o conteúdo usando as APIs GraphQL da AEM. O Cliente Sem Cabeçalho do AEM para JavaScript é usado para executar consultas GraphQL que alimentam o aplicativo.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 4%

---


# React App

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). É fornecido um aplicativo React que demonstra como consultar o conteúdo usando as APIs GraphQL da AEM. O Cliente Sem Cabeçalho do AEM para JavaScript é usado para executar consultas GraphQL que alimentam o aplicativo.

![React Application](./assets/react-screenshot.png)

Um tutorial passo a passo está disponível [here](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo foi projetado para se conectar a um AEM **Autor** ou **Publicar** com a versão mais recente do [Site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) instalado.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=pt-BR)

Recomendamos [implantação do site de referência WKND em um ambiente Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Uma configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) ou [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) também pode ser usado.

## Como usar

1. Clonar o `aem-guides-wknd-graphql` repositório:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd/react-app/.env.development` e garanta que `REACT_APP_HOST_URI` aponta para a instância do target AEM. Atualize o método de autenticação (se estiver se conectando a uma instância do autor).

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
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

Abaixo está um breve resumo dos arquivos e códigos importantes usados para potencializar o aplicativo. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### Cliente autônomo do AEM para JavaScript

O [Cliente Sem Cabeça de AEM](https://github.com/adobe/aem-headless-client-js) é usada para executar a consulta GraphQL. O Cliente Sem Cabeça do AEM fornece dois métodos para executar consultas, [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) e [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` O executa uma consulta GraphQL padrão para AEM conteúdo e é o tipo mais comum de execução de query.

[Consultas Persistentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) são um recurso no AEM que armazena em cache os resultados de uma consulta GraphQL e disponibiliza o resultado pelo GET. Consultas persistentes devem ser usadas para consultas comuns que serão executadas repetidamente. Nesse aplicativo, a lista de Aventuras é a primeira query executada na tela inicial. Esta será uma consulta muito popular e, portanto, uma consulta persistente deve ser usada. `runPersistedQuery` O executa uma solicitação em relação a um endpoint de query persistente.

`src/api/useGraphQL.js` é um [Gancho do efeito de reação](https://reactjs.org/docs/hooks-overview.html#effect-hook) que escuta alterações no parâmetro `query` e `path`. If `query` estiver em branco, uma consulta persistente será usada com base na variável `path`. Este é o local onde o Cliente Sem Cabeçalho do AEM é construído e usado para buscar dados.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Conteúdo de publicidade

O aplicativo exibe principalmente uma lista de Aventuras e dá aos usuários a opção de clicar nos detalhes de uma Aventura.

`Adventures.js` - Exibe uma lista de cartões de Aventuras.