---
title: Gerar token de acesso de servidor para servidor na ação do App Builder
description: Saiba como gerar um token de acesso usando credenciais OAuth de servidor para servidor para usar em uma ação do App Builder.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Gerar token de acesso de servidor para servidor na ação do App Builder

Talvez as ações do App Builder precisem interagir com as APIs do Adobe que oferecem suporte às **credenciais de servidor para servidor do OAuth** e que estão associadas aos projetos do Adobe Developer Console nos quais o aplicativo App Builder está implantado.

Este guia explica como gerar um token de acesso usando as _credenciais de Servidor para Servidor do OAuth_ para usar em uma ação do App Builder.

>[!IMPORTANT]
>
> As credenciais da Conta de serviço (JWT) foram substituídas pelas credenciais de servidor para servidor do OAuth. No entanto, ainda há algumas APIs do Adobe que só oferecem suporte às credenciais da Conta de serviço (JWT) e a migração para o servidor OAuth está em andamento. Consulte a documentação da API do Adobe para entender quais credenciais são compatíveis.

## Configurações de projeto do Adobe Developer Console

Ao adicionar a API do Adobe desejada ao projeto do Adobe Developer Console, na etapa _Configurar API_, selecione o tipo de autenticação **Servidor para servidor OAuth**.

![Adobe Developer Console - Servidor a Servidor OAuth](./assets/s2s-auth/oauth-server-to-server.png)

Para atribuir a conta de serviço de integração criada automaticamente acima, selecione o perfil de produto desejado. Assim, por meio do perfil do produto, as permissões da conta de serviço são controladas.

![Adobe Developer Console - Perfil de Produto](./assets/s2s-auth/select-product-profile.png)

## arquivo .env

No arquivo `.env` do projeto do App Builder, anexe chaves personalizadas para as credenciais de servidor para servidor do OAuth do projeto do Adobe Developer Console. Os valores de credencial de servidor para servidor do OAuth podem ser obtidos das __Credenciais__ > __Servidor para servidor do OAuth__ do projeto do Adobe Developer Console para um determinado espaço de trabalho.

![Credenciais de servidor para servidor do Adobe Developer Console OAuth](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

Os valores de `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` podem ser copiados diretamente da tela de Credenciais de Servidor para Servidor OAuth do projeto do Adobe Developer Console.

## Mapeamento de entradas

Com o valor da credencial de servidor para servidor OAuth definido no arquivo `.env`, ele deve ser mapeado para entradas de ação do AppBuilder para que possa ser lido na própria ação. Para fazer isso, adicione entradas para cada variável na ação `ext.config.yaml` `inputs` no formato: `PARAMS_INPUT_NAME: $ENV_KEY`.

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

As chaves definidas em `inputs` estão disponíveis no objeto `params` fornecido para a ação App Builder.

## Credenciais de servidor para servidor do OAuth para acessar o token

Na ação do App Builder, as credenciais de Servidor para Servidor do OAuth estão disponíveis no objeto `params`. Usando essas credenciais, o token de acesso pode ser gerado usando [bibliotecas do OAuth 2.0](https://oauth.net/code/). Ou você pode usar a [biblioteca de Busca de Nós](https://www.npmjs.com/package/node-fetch) para fazer uma solicitação POST para o ponto de extremidade do token do Adobe IMS para obter o token de acesso.

O exemplo a seguir demonstra como usar a biblioteca `node-fetch` para fazer uma solicitação POST para o ponto de extremidade do token do Adobe IMS para obter o token de acesso.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
