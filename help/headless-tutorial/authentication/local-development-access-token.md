---
title: Token de acesso de desenvolvimento local
description: AEM tokens de acesso ao desenvolvimento local são usados para acelerar o desenvolvimento de integrações com AEM as a Cloud Service que interagem programaticamente com os serviços de autor ou publicação do AEM por HTTP.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Token de acesso de desenvolvimento local

Os desenvolvedores que criam integrações que exigem acesso programático a AEM as a Cloud Service precisam de uma maneira simples e rápida de obter tokens de acesso temporários para AEM para facilitar as atividades de desenvolvimento local. Para atender a essa necessidade, AEM Console do desenvolvedor permite que os desenvolvedores gerem automaticamente tokens de acesso temporários que podem ser usados para acessar AEM de forma programática.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Gerar um token de acesso de desenvolvimento local

![Obter um token de acesso de desenvolvimento local](assets/local-development-access-token/getting-a-local-development-access-token.png)

O Token de acesso de desenvolvimento local fornece acesso aos serviços de autor e publicação do AEM como o usuário que gerou o token, juntamente com suas permissões. Apesar de ser um token de desenvolvimento, não compartilhe esse token ou armazene no controle de origem.

1. Em [Adobe Admin Console](https://adminconsole.adobe.com/) verifique se você, o desenvolvedor, é membro de:
   + __Cloud Manager - Desenvolvedor__ Perfil de produto IMS (concede acesso ao Console do desenvolvedor AEM)
   + Na variável __Administradores de AEM__ ou __Usuários AEM__ Perfil de produto IMS para o serviço do ambiente de AEM, o token de acesso se integra ao
   + O ambiente de sandbox AEM as a Cloud Service requer apenas a associação no __Administradores de AEM__ ou __Usuários AEM__ Perfil de produto
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente as a Cloud Service AEM para integrar o
1. Toque no __elipse__ ao lado do ambiente na __Ambientes__ e selecione __Console do desenvolvedor__
1. Toque em __Integrações__ guia
1. Toque no __Token local__ guia
1. Toque __Obter Token de Desenvolvimento Local__ botão
1. Toque em __botão de download__ no canto superior esquerdo para baixar o arquivo JSON que contém `accessToken` e salve o arquivo JSON em um local seguro na máquina de desenvolvimento.
   + Esse é seu token de acesso do desenvolvedor de 24 horas para o ambiente as a Cloud Service AEM.

![Console do desenvolvedor do AEM - Integrações - Obter token de desenvolvimento local](./assets/local-development-access-token/developer-console.png)

## Usado o token de acesso de desenvolvimento local{#use-local-development-access-token}

![Token de Acesso de Desenvolvimento Local - Aplicativo Externo](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Baixe o token temporário de acesso de desenvolvimento local no Console do desenvolvedor do AEM
   + O token de acesso de desenvolvimento local expira a cada 24 horas, portanto, os desenvolvedores precisam fazer o download de novos tokens de acesso diariamente
1. Está sendo desenvolvido um Aplicativo Externo que interage programaticamente com AEM as a Cloud Service
1. O Aplicativo Externo lê no Token de Acesso de Desenvolvimento Local
1. O Aplicativo Externo constrói solicitações HTTP para AEM as a Cloud Service, adicionando o Token de Acesso de Desenvolvimento Local como um token Portador ao cabeçalho de Autorização das solicitações HTTP
1. AEM as a Cloud Service recebe a solicitação HTTP, autentica a solicitação e executa o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP ao Aplicativo Externo

### O Aplicativo Externo De Amostra

Criaremos um aplicativo JavaScript externo simples para ilustrar como acessar programaticamente AEM as a Cloud Service por HTTPS usando o token de acesso do desenvolvedor local. Isso ilustra como _any_ o aplicativo ou sistema em execução fora do AEM, independentemente da estrutura ou idioma, pode usar o token de acesso para autenticar e acessar de forma programática AEM as a Cloud Service. No [próxima seção](./service-credentials.md), atualizaremos esse código de aplicativo para dar suporte à abordagem de geração de um token para uso da produção.

Este aplicativo de amostra é executado a partir da linha de comando e atualiza AEM metadados de ativos usando APIs HTTP AEM Assets, usando o seguinte fluxo:

1. Leituras em parâmetros da linha de comando (`getCommandLineParams()`)
1. Obtém o token de acesso usado para autenticação para AEM as a Cloud Service (`getAccessToken(...)`)
1. Lista todos os ativos em uma pasta de ativos AEM especificada em um parâmetro de linha de comando (`listAssetsByFolder(...)`)
1. Atualize os metadados dos ativos listados com valores especificados nos parâmetros de linha de comando (`updateMetadata(...)`)

O elemento principal na autenticação programática para AEM usando o token de acesso está adicionando um cabeçalho de solicitação HTTP de autorização a todas as solicitações HTTP feitas ao AEM, no seguinte formato:

+ `Authorization: Bearer ACCESS_TOKEN`

## Execução do aplicativo externo

1. Certifique-se de que [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) O é instalado no computador de desenvolvimento local, que é usado para executar o aplicativo externo
1. Baixe e descompacte o [aplicativo externo de amostra](./assets/aem-guides_token-authentication-external-application.zip)
1. Na linha de comando, na pasta deste projeto, execute `npm install`
1. Copie o [baixado o token de acesso ao desenvolvimento local](#download-local-development-access-token) para um arquivo nomeado `local_development_token.json` na raiz do projeto
   + Mas lembre-se, nunca confira nenhuma credencial ao Git!
1. Abrir `index.js` e revise o código e os comentários externos do aplicativo.

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

   Revise o `fetch(..)` invocações no `listAssetsByFolder(...)` e `updateMetadata(...)`e aviso `headers` defina as `Authorization` Cabeçalho de solicitação HTTP com um valor de `Bearer ACCESS_TOKEN`. É assim que a solicitação HTTP originada do aplicativo externo é autenticada para AEM as a Cloud Service.

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

   Qualquer solicitação HTTP para AEM as a Cloud Service deve definir o token de acesso do Portador no cabeçalho Autorização. Lembre-se, cada ambiente AEM as a Cloud Service requer seu próprio token de acesso. O token de acesso do desenvolvimento não funciona em Estágio ou Produção, o de Estágio não funciona em Desenvolvimento ou Produção e o de Produção não funciona em Desenvolvimento ou Estágio!

1. Usando a linha de comando, na raiz do projeto, execute o aplicativo, transmitindo os seguintes parâmetros:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Os seguintes parâmetros são passados em:

   + `aem`: O esquema e o nome do host do ambiente as a Cloud Service AEM com o qual o aplicativo interage (por exemplo, `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: O caminho da pasta de ativos cujos ativos são atualizados com o `propertyValue`; NÃO adicione o `/content/dam` prefixo (por exemplo, `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: O nome da propriedade do ativo a ser atualizado, em relação a `[dam:Asset]/jcr:content` (ex. `metadata/dc:rights`).
   + `propertyValue`: O valor para definir a variável `propertyName` a; valores com espaços precisam ser encapsulados com `"` (ex. `"WKND Limited Use"`)
   + `file`: O caminho de arquivo relativo para o arquivo JSON baixado do Console do desenvolvedor AEM.

   Uma execução bem-sucedida da saída dos resultados do aplicativo para cada ativo atualizado:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Verificar atualização de metadados no AEM

Verifique se os metadados foram atualizados, fazendo logon no ambiente as a Cloud Service AEM (verifique se o mesmo host foi passado para a função `aem` é acessado o parâmetro da linha de comando).

1. Faça logon no ambiente as a Cloud Service AEM com o qual o aplicativo externo interagiu (use o mesmo host fornecido no `aem` parâmetro da linha de comando)
1. Navegue até o __Ativos__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pela `folder` parâmetro da linha de comando, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Brinde de Vinho Napa__
1. Abra o __Propriedades__ para qualquer ativo (que não seja Fragmento de conteúdo) na pasta
1. Toque em __Avançado__ guia
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que é mapeado para o `metadata/dc:rights` Propriedade JCR, que reflete o valor fornecido na variável `propertyValue` , por exemplo __Utilização limitada WKND__

![Atualização de metadados de uso limitado WKND](./assets/local-development-access-token/asset-metadata.png)

## Próximas etapas

Agora que acessamos de forma programática AEM as a Cloud Service usando o token de desenvolvimento local. Em seguida, precisamos atualizar o aplicativo para lidar com o uso de Credenciais de serviço, para que esse aplicativo possa ser usado em um contexto de produção.

+ [Como usar credenciais de serviço](./service-credentials.md)
