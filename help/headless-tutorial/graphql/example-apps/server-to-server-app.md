---
title: Aplicativo Node.js de servidor para servidor - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Node.js do lado do servidor demonstra como consultar conteúdo usando APIs do GraphQL AEM usando consultas persistentes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 6%

---

# Aplicativo Node.js de servidor para servidor

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Esse aplicativo servidor a servidor demonstra como consultar conteúdo usando APIs do GraphQL AEM usando consultas persistentes e imprimi-lo no terminal.

![Aplicativo Node.js de servidor para servidor com AEM headless](./assets/server-to-server-app/server-to-server-app.png)

Visualize o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Requisitos AEM

O aplicativo Node.js funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem o [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) a ser instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=pt-BR)
+ Opcionalmente, [credenciais de serviço](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) se autorizar solicitações (por exemplo, conectar-se ao serviço de autor do AEM).

Este aplicativo Node.js pode se conectar ao AEM Author ou AEM Publish com base nos parâmetros de linha de comando.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra um terminal e execute os comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. O aplicativo pode ser executado usando o comando:

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Por exemplo, para executar o aplicativo em relação à publicação do AEM sem autorização:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Para executar o aplicativo em relação ao AEM Author com autorização:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Uma lista JSON de aventuras do site de referência WKND deve ser impressa no terminal.

## O código

Abaixo está um resumo de como o aplicativo Node.js servidor para servidor é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

O caso de uso comum para aplicativos headless de AEM de servidor para servidor é sincronizar os dados do Fragmento de conteúdo de AEM para outros sistemas, no entanto, esse aplicativo é intencionalmente simples e imprime os resultados JSON da consulta persistente.

### Consultas persistentes

Seguindo AEM práticas recomendadas headless, o aplicativo usa consultas persistentes AEM GraphQL para consultar dados de aventuras. O aplicativo usa duas consultas persistentes:

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

### Criar AEM cliente headless

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### Executar consulta persistente do GraphQL

AEM consultas persistentes são executadas pelo HTTP GET e, portanto, o [AEM cliente headless para Node.js](https://github.com/adobe/aem-headless-client-nodejs) é usado para [executar as consultas persistentes do GraphQL](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) contra AEM e recupera o conteúdo da aventura.

A consulta persistente é invocada ao chamar `aemHeadlessClient.runPersistedQuery(...)`e transmitindo o nome de consulta persistente do GraphQL. Depois que o GraphQL retornar os dados, passe-os para o `doSomethingWithDataFromAEM(..)` , que imprime os resultados, mas normalmente envia os dados para outro sistema, ou gera alguma saída com base nos dados recuperados.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
