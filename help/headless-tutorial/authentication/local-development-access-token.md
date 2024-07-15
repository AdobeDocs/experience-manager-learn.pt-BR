---
title: Token de acesso de desenvolvimento local
description: Os tokens de acesso de desenvolvimento local do AEM são usados para acelerar o desenvolvimento de integrações com o AEM as a Cloud Service que interage programaticamente com os serviços do AEM Author ou do Publish por HTTP.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 572
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Token de acesso de desenvolvimento local

Os desenvolvedores que criam integrações que exigem acesso programático ao AEM as a Cloud Service precisam de uma maneira simples e rápida de obter tokens de acesso temporários para AEM a fim de facilitar as atividades de desenvolvimento locais. Para atender a essa necessidade, o Developer Console do AEM permite que os desenvolvedores gerem tokens de acesso temporários que podem ser usados para acessar o AEM de forma programática.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Gerar um token de acesso de desenvolvimento local

![Obtendo um Token de Acesso de Desenvolvimento Local](assets/local-development-access-token/getting-a-local-development-access-token.png)

O token de acesso de desenvolvimento local fornece acesso aos serviços do AEM Author e do Publish como o usuário que gerou o token, juntamente com suas permissões. Apesar de este ser um token de desenvolvimento, não compartilhe este token ou armazene no controle do código-fonte.

1. No [Adobe Admin Console](https://adminconsole.adobe.com/), verifique se você, o desenvolvedor, é membro de:
   + __Cloud Manager - Desenvolvedor__ Perfil de Produto IMS (concede acesso ao AEM Developer Console)
   + O Perfil de Produto IMS dos __Administradores do AEM__ ou dos __Usuários do AEM__ para o serviço do ambiente AEM ao qual o token de acesso se integra
   + O ambiente AEM as a Cloud Service de sandbox exige apenas a associação no Perfil do produto __Administradores do AEM__ ou __Usuários do AEM__
1. Faça logon no [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente do AEM as a Cloud Service para integrar com o
1. Toque nas __reticências__ ao lado do ambiente na seção __Ambientes__ e selecione __Developer Console__
1. Toque na guia __Integrações__
1. Toque na guia __Token local__
1. Toque no botão __Obter token de desenvolvimento local__
1. Toque no __botão de download__ no canto superior esquerdo para baixar o arquivo JSON que contém o valor `accessToken` e salvar o arquivo JSON em um local seguro na máquina de desenvolvimento.
   + Este é o seu token de acesso de desenvolvedor 24 horas para o ambiente do AEM as a Cloud Service.

![Developer Console AEM - Integrações - Obter Token De Desenvolvimento Local](./assets/local-development-access-token/developer-console.png)

## Usado o token de acesso de desenvolvimento local{#use-local-development-access-token}

![Token de acesso de desenvolvimento local - Aplicativo externo](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Baixe o token de acesso de desenvolvimento local temporário do AEM Developer Console
   + O token de acesso de desenvolvimento local expira a cada 24 horas, portanto, os desenvolvedores precisam baixar novos tokens de acesso diariamente
1. Está sendo desenvolvido um aplicativo externo que interage programaticamente com o AEM as a Cloud Service
1. O aplicativo externo lê no token de acesso de desenvolvimento local
1. O aplicativo externo constrói solicitações HTTP para o AEM as a Cloud Service, adicionando o token de acesso de desenvolvimento local como um token de portador ao cabeçalho de autorização das solicitações HTTP
1. O AEM as a Cloud Service recebe a solicitação HTTP, autentica a solicitação e realiza o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP de volta ao Aplicativo externo

### O Aplicativo Externo de Exemplo

Criaremos um aplicativo JavaScript externo simples para ilustrar como acessar programaticamente o AEM as a Cloud Service por HTTPS usando o token de acesso do desenvolvedor local. Isso ilustra como _qualquer_ aplicativo ou sistema em execução fora do AEM, independentemente da estrutura ou linguagem, pode usar o token de acesso para autenticar e acessar programaticamente o AEM as a Cloud Service. Na [próxima seção](./service-credentials.md), atualizaremos este código de aplicativo para dar suporte à abordagem de geração de token para uso de produção.

Esse aplicativo de amostra é executado a partir da linha de comando e atualiza os metadados de ativos AEM usando APIs HTTP do AEM Assets, usando o seguinte fluxo:

1. Leituras em parâmetros da linha de comando (`getCommandLineParams()`)
1. Obtém o token de acesso usado para autenticar no AEM as a Cloud Service (`getAccessToken(...)`)
1. Lista todos os ativos em uma pasta de ativos AEM especificada em parâmetros de linha de comando (`listAssetsByFolder(...)`)
1. Atualizar os metadados dos ativos listados com valores especificados nos parâmetros de linha de comando (`updateMetadata(...)`)

O elemento principal na autenticação programática para o AEM usando o token de acesso é adicionar um cabeçalho de solicitação HTTP de autorização a todas as solicitações HTTP feitas no AEM, no seguinte formato:

+ `Authorization: Bearer ACCESS_TOKEN`

## Executando o aplicativo externo

1. Verifique se o [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) está instalado na máquina de desenvolvimento local, que é usada para executar o aplicativo externo
1. Baixe e descompacte o [aplicativo externo de amostra](./assets/aem-guides_token-authentication-external-application.zip)
1. Na linha de comando, na pasta deste projeto, execute `npm install`
1. Copie o [Token de Acesso de Desenvolvimento Local](#download-local-development-access-token) baixado para um arquivo denominado `local_development_token.json` na raiz do projeto
   + Mas lembre-se, nunca confirme credenciais para o Git!
1. Abra `index.js` e revise o código de aplicativo externo e os comentários.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   Revise as invocações `fetch(..)` em `listAssetsByFolder(...)` e `updateMetadata(...)`, e observe `headers` defina o cabeçalho de solicitação HTTP `Authorization` com um valor de `Bearer ACCESS_TOKEN`. É assim que a solicitação HTTP proveniente do aplicativo externo é autenticada no AEM as a Cloud Service.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   Qualquer solicitação HTTP para o AEM as a Cloud Service deve definir o token de acesso do portador no cabeçalho Autorização. Lembre-se de que cada ambiente do AEM as a Cloud Service requer seu próprio token de acesso. O token de acesso do desenvolvimento não funciona no preparo ou na produção, o do preparo não funciona no desenvolvimento ou na produção e o da produção não funciona no desenvolvimento ou no preparo!

1. Usando a linha de comando, na raiz do projeto, execute a aplicação, transmitindo os seguintes parâmetros:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Os seguintes parâmetros são transmitidos em:

   + `aem`: O esquema e o nome de host do ambiente AEM as a Cloud Service com o qual o aplicativo interage (por exemplo, `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: o caminho da pasta de ativos cujos ativos são atualizados com o `propertyValue`; NÃO adicione o prefixo `/content/dam` (ex. `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: O nome da propriedade do ativo a ser atualizado, relativo a `[dam:Asset]/jcr:content` (ex. `metadata/dc:rights`).
   + `propertyValue`: O valor para o qual definir `propertyName`; valores com espaços precisam ser encapsulados com `"` (ex. `"WKND Limited Use"`)
   + `file`: o caminho relativo para o arquivo JSON baixado do AEM Developer Console.

   Uma execução bem-sucedida da saída de resultados do aplicativo para cada ativo atualizado:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Verificar atualização de metadados no AEM

Verifique se os metadados foram atualizados, fazendo logon no ambiente do AEM as a Cloud Service (verifique se o mesmo host passado para o parâmetro de linha de comando `aem` foi acessado).

1. Faça logon no ambiente do AEM as a Cloud Service com o qual o aplicativo externo interagiu (use o mesmo host fornecido no parâmetro de linha de comando `aem`)
1. Navegue até __Assets__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pelo parâmetro de linha de comando `folder`, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Napa Wine Tasting__
1. Abra as __Propriedades__ de qualquer ativo (que não seja Fragmento de Conteúdo) na pasta
1. Toque na guia __Avançado__
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que está mapeada para a propriedade JCR `metadata/dc:rights` atualizada, que reflete o valor fornecido no parâmetro `propertyValue`, por exemplo __Uso Limitado de WKND__

![Atualização de Metadados de Uso Limitado do WKND](./assets/local-development-access-token/asset-metadata.png)

## Próximas etapas

Agora que acessamos programaticamente o AEM as a Cloud Service usando o token de desenvolvimento local. Em seguida, precisamos atualizar o aplicativo para lidar com o uso de Credenciais de serviço, para que esse aplicativo possa ser usado em um contexto de produção.

+ [Como usar as Credenciais de serviço](./service-credentials.md)
