---
title: Gerar token de acesso na ação do App Builder
description: Saiba como gerar um token de acesso usando credenciais JWT para uso em uma ação do App Builder.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 40679e80fd9270dd9fad8174a986fd1fdd5e3d29
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 1%

---


# Gerar token de acesso na ação do App Builder

As ações do App Builder podem precisar interagir com as APIs do Adobe associadas aos projetos do Adobe Developer Console, pois o aplicativo do App Builder também foi implantado.

Isso pode exigir que a ação do App Builder gere seu próprio token de acesso associado ao projeto desejado do Adobe Developer Console.

>[!IMPORTANT]
>
> Revisão [Documentação de segurança do App Builder](https://developer.adobe.com/app-builder/docs/guides/security/) para entender quando é apropriado gerar tokens de acesso em vez de usar os tokens de acesso fornecidos.
>
> A ação personalizada pode precisar fornecer suas próprias verificações de segurança para garantir que somente os consumidores permitidos possam acessar a ação App Builder e os serviços da Adobe por trás dela.


## arquivo .env

No projeto do App Builder `.env` anexe chaves personalizadas para cada uma das credenciais JWT do projeto do Adobe Developer Console. Os valores da credencial JWT podem ser obtidos do projeto do Console do Adobe Developer __Credenciais__ > __Conta de serviço (JWT)__ para um determinado espaço de trabalho.

![Credenciais do Serviço JWT do Console Adobe Developer](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

Os valores para `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` pode ser copiado diretamente da tela Credenciais JWT do projeto do Console do Adobe Developer.

### Metascópios

Determine as APIs do Adobe e seus metascópios com os quais a ação do App Builder interage. Listar metascópios com delimitadores de vírgula no `JWT_METASCOPES` chave. Os metascópios válidos são listados em [Documentação do JWT](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


Por exemplo, o seguinte valor pode ser adicionado ao `JWT_METASCOPES` na chave `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Chave privada

O `JWT_PRIVATE_KEY` deve ser formatado especialmente, pois é nativamente um valor de várias linhas, que não é compatível com `.env` arquivos. A maneira mais fácil é codificar a chave privada com base64. Codificação da chave privada em Base64 (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) pode ser feito usando ferramentas nativas fornecidas pelo sistema operacional.

>[!BEGINTABS]

>[!TAB macOS]

1. Abrir `Terminal`
1. Executar o comando `base64 -i /path/to/private.key | pbcopy`
1. A saída base64 é copiada automaticamente para a área de transferência
1. Colar em `.env` como valor para a chave correspondente

>[!TAB Windows]

1. Abrir `Command Prompt`
1. Executar o comando `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. Executar o comando `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. Copie a saída base64 para a área de transferência
1. Colar em `.env` como valor para a chave correspondente

>[!TAB Linux®]

1. Abrir terminal
1. Executar o comando `base64 private.key`
1. Copie a saída base64 para a área de transferência
1. Colar em `.env` como valor para a chave correspondente

>[!ENDTABS]

Por exemplo, a seguinte chave privada codificada em base64 pode ser adicionada à variável `JWT_PRIVATE_KEY` na chave `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Mapeamento de entradas

Com o valor da credencial JWT definido na variável `.env` , elas devem ser mapeadas para entradas de ação do AppBuilder para que possam ser lidas na própria ação. Para fazer isso, adicione entradas para cada variável na variável `ext.config.yaml` ação `inputs` no formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

Por exemplo:

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

As chaves definidas em `inputs` estão disponíveis no `params` objeto fornecido para a ação App Builder.


## Credenciais JWT para acessar o token

Na ação do App Builder, as credenciais do JWT estão disponíveis na variável `params` e utilizável por [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) para gerar um token de acesso, que, por sua vez, pode acessar outras APIs e serviços do Adobe.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
