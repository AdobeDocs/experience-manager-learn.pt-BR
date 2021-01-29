---
title: Token de acesso de desenvolvimento local
description: AEM Tokens de acesso de desenvolvimento local são usados para acelerar o desenvolvimento de integrações com AEM como um Cloud Service que interage programaticamente com os serviços de autor ou publicação do AEM por HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
translation-type: tm+mt
source-git-commit: c4f3d437b5ecfe6cb97314076cd3a5e31b184c79
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---


# Token de acesso de desenvolvimento local

Os desenvolvedores que criam integrações que exigem acesso programático a AEM como Cloud Service precisam de uma maneira simples e rápida de obter tokens de acesso temporários para AEM para facilitar as atividades de desenvolvimento local. Para atender a essa necessidade, AEM Developer Console permite que os desenvolvedores gerem tokens de acesso temporários que podem ser usados para acessar AEM de forma programática.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## Gerar um Token de acesso de desenvolvimento local

![Obtendo um Token de acesso de desenvolvimento local](assets/local-development-access-token/getting-a-local-development-access-token.png)

O Token de acesso de Desenvolvimento local fornece acesso aos serviços de autor e publicação do AEM como o usuário que gerou o token, juntamente com suas permissões. Apesar de ser um token de desenvolvimento, não compartilhe esse token nem armazene no controle de origem.

1. Em [AdminConsole](https://adminconsole.adobe.com/) Adobe, verifique se você, o desenvolvedor, é membro de:
   + __Gerenciador de nuvem - Perfil de produto__ DeveloperIMS (concede acesso ao AEM Developer Console)
   + O Perfil de Produto IMS __AEM Administradores__ ou __AEM Usuários__ para o serviço de ambiente AEM que o token de acesso integrará
   + A AEM Sandbox como ambientes Cloud Service só exige a associação no Perfil de produtos __AEM Administradores__ ou __AEM Usuários__
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o AEM como um ambiente Cloud Service para integrar com
1. Toque em __elipse__ ao lado do ambiente na seção __Ambientes__ e selecione __Developer Console__
1. Toque na guia __Integrações__
1. Toque no botão __Obter token de desenvolvimento local__
1. Toque no __botão de download__ no canto superior esquerdo para baixar o arquivo JSON que contém o valor `accessToken` e salve o arquivo JSON em um local seguro no computador de desenvolvimento.
   + Este é o seu token de acesso de 24 horas para o desenvolvedor como um ambiente.

![Console do desenvolvedor do AEM - Integrações - Obter token de desenvolvimento local](./assets/local-development-access-token/developer-console.png)

## Usado o Token de acesso de Desenvolvimento Local{#use-local-development-access-token}

![Token de acesso de desenvolvimento local - Aplicativo externo](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Baixe o Token de acesso temporário de Desenvolvimento local no AEM Developer Console
   + O Token de acesso de desenvolvimento local expira a cada 24 horas, portanto os desenvolvedores precisarão fazer o download de novos tokens de acesso diariamente
1. Está sendo desenvolvido um aplicativo externo que interage programaticamente com AEM como Cloud Service
1. O aplicativo externo lê no Token de acesso de desenvolvimento local
1. O Aplicativo externo constrói solicitações HTTP para AEM como um Cloud Service, adicionando o Token de acesso de Desenvolvimento local como um token de Portador ao cabeçalho de Autorização das solicitações HTTP
1. AEM como um Cloud Service recebe a solicitação HTTP, autentica a solicitação e executa o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP para o Aplicativo Externo

### O aplicativo externo de amostra

Criaremos um aplicativo JavaScript externo simples para ilustrar como acessar AEM de forma programática como um Cloud Service sobre HTTPS usando o token de acesso de desenvolvedor local. Isso ilustra como _qualquer_ aplicativo ou sistema em execução fora do AEM, independentemente da estrutura ou do idioma, pode usar o token de acesso para autenticar e acessar programaticamente AEM como um Cloud Service. Na [próxima seção](./service-credentials.md), atualizaremos este código de aplicativo para suportar a abordagem de geração de um token para uso na produção.

Este aplicativo de amostra é executado da linha de comando e atualiza AEM metadados de ativos usando APIs HTTP AEM Assets, usando o seguinte fluxo:

1. Lê nos parâmetros da linha de comando (`getCommandLineParams()`)
1. Obtém o token de acesso usado para autenticação para AEM como Cloud Service (`getAccessToken(...)`)
1. Lista todos os ativos em uma pasta de ativos AEM especificada em parâmetros de linha de comando (`listAssetsByFolder(...)`)
1. Atualizar metadados de ativos listados com valores especificados nos parâmetros da linha de comando (`updateMetadata(...)`)

O elemento principal na autenticação programática para AEM usando o token de acesso está adicionando um cabeçalho de solicitação HTTP de autorização a todas as solicitações HTTP feitas ao AEM, no seguinte formato:

+ `Authorization: Bearer ACCESS_TOKEN`

## Execução do aplicativo externo

1. Verifique se [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) está instalado no computador de desenvolvimento local, que será usado para executar o aplicativo externo
1. Baixe e descompacte o [aplicativo externo de amostra](./assets/aem-guides_token-authentication-external-application.zip)
1. Na linha de comando, na pasta deste projeto, execute `npm install`
1. Copie o [Token de acesso de Desenvolvimento Local](#download-local-development-access-token) para um arquivo chamado `local_development_token.json` na raiz do projeto
   + Mas lembre-se, nunca confirme nada ao Git!
1. Abra `index.js` e reveja o código e os comentários externos do aplicativo.

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
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

   Revise as invocações `fetch(..)` em `listAssetsByFolder(...)` e `updateMetadata(...)`, e observe `headers` definir o cabeçalho `Authorization` da solicitação HTTP com um valor de `Bearer ACCESS_TOKEN`. É assim que a solicitação HTTP proveniente do aplicativo externo é autenticada para AEM como Cloud Service.

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

   Quaisquer solicitações HTTP para AEM como Cloud Service, devem definir o token de acesso do Portador no cabeçalho Autorização. Lembre-se, cada AEM como ambiente Cloud Service requer seu próprio token de acesso. O token de acesso do desenvolvimento não funcionará em Estágio ou Produção, o Estágio não funcionará em Desenvolvimento ou Produção, e o da Produção não funcionará em Desenvolvimento ou Estágio!

1. Usando a linha de comando, na raiz do projeto execute o aplicativo, transmitindo os seguintes parâmetros:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   Os seguintes parâmetros são passados em:

   + `aem`: O esquema e o nome do host do AEM como um ambiente com o qual o aplicativo interagirá (por exemplo,  `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: O caminho da pasta de ativos cujos ativos serão atualizados com o  `propertyValue`; não adicione o  `/content/dam` prefixo (ex.  `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`: O nome da propriedade do ativo a ser atualizada, em relação a  `[dam:Asset]/jcr:content` (ex.  `metadata/dc:rights`).
   + `propertyValue`: O valor a ser definido  `propertyName` como; valores com espaços precisam ser encapsulados com  `"` (ex.  `"WKND Limited Use"`)
   + `file`: O caminho do arquivo relativo para o arquivo JSON baixado do AEM Developer Console.

   Uma execução bem-sucedida da saída dos resultados do aplicativo para cada ativo foi atualizada:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Verificar atualização de metadados no AEM

Verifique se os metadados foram atualizados fazendo logon no AEM como um ambiente Cloud Service (verifique se o mesmo host passado para o parâmetro de linha de comando `aem` é acessado).

1. Faça logon no AEM como um ambiente Cloud Service com o qual o aplicativo externo interagiu (use o mesmo host fornecido no parâmetro de linha de comando `aem`)
1. Navegue até __Ativos__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pelo parâmetro de linha de comando `folder`, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Napa Vinculação__
1. Abra o __Propriedades__ para qualquer ativo (que não seja Fragmento de conteúdo) na pasta
1. Toque na guia __Avançado__
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que está mapeada para a propriedade JCR `metadata/dc:rights` atualizada, que reflete o valor fornecido no parâmetro `propertyValue`, por exemplo __WKND Limited Use__

![Atualização de metadados de uso limitado WKND](./assets/local-development-access-token/asset-metadata.png)

## Próximos passos

Agora que acessamos de forma programática AEM como um Cloud Service usando o token de desenvolvimento local, precisamos atualizar o aplicativo para lidar com o uso de Credenciais de serviço, para que esse aplicativo possa ser usado em um contexto de produção.

+ [Como usar as credenciais de serviço](./service-credentials.md)
