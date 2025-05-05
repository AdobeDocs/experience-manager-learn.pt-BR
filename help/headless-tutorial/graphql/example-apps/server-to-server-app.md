---
title: Aplicativo Node.js de servidor para servidor - Exemplo do AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Node.js do lado do servidor demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 135
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Aplicativo Node.js de servidor para servidor

Aplicativos de exemplo são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo de servidor para servidor demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes e imprimi-las no terminal.

![Aplicativo Node.js de servidor para servidor com AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo Node.js funciona com as seguintes opções de implantação do AEM. Todas as implantações exigem que o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) esteja instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=pt-BR)
+ Opcionalmente, [credenciais de serviço](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=pt-BR), se estiver autorizando solicitações (por exemplo, conectando-se ao serviço do AEM Author).

Este aplicativo Node.js pode se conectar ao AEM Author ou ao AEM Publish com base nos parâmetros da linha de comando.

## Como usar

1. Clonar o repositório `adobe/aem-guides-wknd-graphql`:

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

   Por exemplo, para executar o aplicativo em relação ao AEM Publish sem autorização:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   Para executar o aplicativo em relação ao AEM Author com autorização:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Uma lista JSON de aventuras do site de referência da WKND deve ser impressa no terminal.

## O código

Abaixo está um resumo de como o aplicativo Node.js servidor a servidor é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

O caso de uso comum para aplicativos headless do AEM de servidor para servidor é sincronizar dados do fragmento de conteúdo do AEM com outros sistemas, no entanto, esse aplicativo é intencionalmente simples e imprime os resultados JSON da consulta persistente.

### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventura da visualização inicial.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

### Criar cliente do AEM Headless

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

As consultas persistentes do AEM são executadas por HTTP GET e, portanto, o [cliente AEM Headless para Node.js](https://github.com/adobe/aem-headless-client-nodejs) é usado para [executar as consultas persistentes do GraphQL](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) no AEM e recuperar o conteúdo de aventura.

A consulta persistente é invocada chamando `aemHeadlessClient.runPersistedQuery(...)` e transmitindo o nome da consulta persistente do GraphQL. Depois que o GraphQL retornar os dados, passe-os para a função simplificada `doSomethingWithDataFromAEM(..)`, que imprime os resultados, mas normalmente enviaria os dados para outro sistema ou geraria alguma saída com base nos dados recuperados.

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
