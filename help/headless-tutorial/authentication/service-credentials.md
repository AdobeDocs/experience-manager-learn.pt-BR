---
title: Credenciais de serviço
description: Saiba como usar as Credenciais de serviço usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação por HTTP.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: f8ed9fddb5f244860ba229b46a80638a7269d95e
workflow-type: tm+mt
source-wordcount: '1925'
ht-degree: 0%

---

# Credenciais de serviço

As integrações com o Adobe Experience Manager (AEM) as a Cloud Service devem ser capazes de autenticar com segurança no serviço de AEM. O Console do desenvolvedor do AEM concede acesso às Credenciais de serviço, que são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

As Credenciais de serviço podem parecer semelhantes [Tokens de acesso de desenvolvimento local](./local-development-access-token.md) mas são diferentes de algumas maneiras principais:

+ As Credenciais de serviço são associadas às Contas técnicas. Várias credenciais de serviço podem estar ativas para uma Conta Técnica.
+ As credenciais de serviço são _não_ tokens de acesso simples, em vez de serem credenciais usadas para _obter_ tokens de acesso.
+ As Credenciais de serviço são mais permanentes (os certificados expiram a cada 365 dias) e não são alteradas, a menos que sejam revogadas, enquanto os Tokens de acesso de desenvolvimento local expiram diariamente.
+ As Credenciais de serviço para um ambiente as a Cloud Service AEM são mapeadas para um único usuário de conta técnica do AEM AEM, enquanto os Tokens de acesso de desenvolvimento local são autenticados como o usuário do que gerou o token de acesso.
+ Um ambiente as a Cloud Service do AEM pode ter até dez contas técnicas, cada uma com suas próprias Credenciais de serviço, cada uma mapeando para um usuário do AEM de conta técnica distinto.

As Credenciais de serviço e os tokens de acesso gerados por elas, além dos Tokens de acesso de desenvolvimento local, devem ser mantidos em segredo. Como todos os três podem ser usados para obter, o acesso ao seu respectivo ambiente as a Cloud Service AEM.

## Gerar credenciais de serviço

A geração de Credenciais de serviço é dividida em duas etapas:

1. Uma criação de conta técnica única por um administrador da organização Adobe IMS
1. O download e o uso das credenciais de serviço da conta técnica JSON

### Criar uma conta técnica

As Credenciais de serviço, diferentemente dos Tokens de acesso de desenvolvimento local, exigem que uma Conta técnica seja criada por um Administrador IMS da Adobe Org antes de serem baixadas. Contas técnicas discretas devem ser criadas para cada cliente que requer acesso programático ao AEM.

![Criar uma conta técnica](assets/service-credentials/initialize-service-credentials.png)

As Contas técnicas são criadas uma vez, no entanto, as Chaves privadas usam o para gerenciar Credenciais de serviço associadas à Conta técnica do podem ser gerenciadas ao longo do tempo. Por exemplo, novas Credenciais de chave privada/serviço devem ser geradas antes da expiração da chave privada atual, para permitir o acesso ininterrupto por um usuário das Credenciais de serviço.

1. Verifique se você está conectado como:
   + __Administrador de sistema da organização IMS da Adobe__
   + Membro do __Administradores de AEM__ Perfil de produto IMS em __AEM Author__
1. Efetue logon no [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente as a Cloud Service AEM para integrar e configure as Credenciais de serviço para
1. Toque nas reticências ao lado do ambiente no __Ambientes__ e selecione __Console do desenvolvedor__
1. Toque no __Integrações__ guia
1. Toque no __Contas técnicas__ guia
1. Toque __Criar nova conta técnica__ botão
1. As Credenciais de serviço da conta técnica são inicializadas e exibidas como JSON

![Console do desenvolvedor do AEM - Integrações - Obter credenciais de serviço](./assets/service-credentials/developer-console.png)

Depois que as Credenciais de serviço do ambiente AEM as Cloud Service forem inicializadas, outros desenvolvedores do AEM em sua organização Adobe IMS poderão baixá-las.

### Baixar credenciais de serviço

![Baixar credenciais de serviço](assets/service-credentials/download-service-credentials.png)

O download das Credenciais de serviço segue as etapas semelhantes à inicialização.

1. Verifique se você está conectado como:
   + __Administrador da organização Adobe IMS__
   + Membro do __Administradores de AEM__ Perfil de produto IMS em __AEM Author__
1. Efetue logon no [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente as a Cloud Service AEM para integrar com o
1. Toque nas reticências ao lado do ambiente no __Ambientes__ e selecione __Console do desenvolvedor__
1. Toque no __Integrações__ guia
1. Toque no __Contas técnicas__ guia
1. Expanda a __Conta técnica__ a ser usado
1. Expanda a __Chave privada__ cujas credenciais de serviço serão baixadas e verificar se o status é __Ativo__
1. Toque no __..__ > __Exibir__ associado à __Chave privada__, que exibe as Credenciais de serviço JSON
1. Toque no botão de download no canto superior esquerdo para baixar o arquivo JSON que contém o valor Credenciais de serviço e salvar o arquivo em um local seguro

## Instalar as Credenciais de Serviço

As Credenciais de serviço fornecem os detalhes necessários para gerar um JWT, que é substituído por um token de acesso usado para autenticação com o AEM as a Cloud Service. As Credenciais de Serviço devem ser armazenadas em um local seguro acessível aos aplicativos, sistemas ou serviços externos que as utilizam para acessar o AEM. Como e onde as Credenciais de serviço são gerenciadas são exclusivas por cliente.

Para simplificar, este tutorial passa as Credenciais de serviço no pela linha de comando. No entanto, trabalhe com sua equipe de Segurança de TI para entender como armazenar e acessar essas credenciais de acordo com as diretrizes de segurança de sua organização.

1. Copie o [baixadas as Credenciais de serviço JSON](#download-service-credentials) para um arquivo chamado `service_token.json` na raiz do projeto
   + Lembre-se, nunca confirmar _quaisquer credenciais_ ao Git!

## Usar credenciais de serviço

As Credenciais de serviço, um objeto JSON totalmente formado, não são as mesmas que o JWT nem o token de acesso. Em vez disso, as Credenciais de serviço (que contêm uma chave privada) são usadas para gerar um JWT, que é substituído por um token de acesso pelas APIs do Adobe IMS.

![Credenciais de Serviço - Aplicativo Externo](assets/service-credentials/service-credentials-external-application.png)

1. Baixe as credenciais de serviço do Console do desenvolvedor do AEM em um local seguro
1. O aplicativo externo precisa interagir programaticamente com o ambiente as a Cloud Service do AEM
1. O Aplicativo Externo lê as Credenciais de Serviço de um local seguro
1. O aplicativo externo usa informações das credenciais de serviço para criar um token JWT
1. O token JWT é enviado ao Adobe IMS para troca por um token de acesso
1. O Adobe IMS retorna um token de acesso que pode ser usado para acessar o AEM as a Cloud Service
   + Os tokens de acesso não podem alterar um tempo de expiração.
1. O aplicativo externo faz solicitações HTTP ao AEM as a Cloud Service, adicionando o token de acesso como um token de portador ao cabeçalho de autorização das solicitações HTTP
1. O AEM as a Cloud Service recebe a solicitação HTTP, autentica a solicitação e realiza o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP de volta ao Aplicativo externo

### Atualizações no aplicativo externo

Para acessar o AEM as a Cloud Service usando as Credenciais de serviço do, o aplicativo externo deve ser atualizado de três maneiras:

1. Ler nas Credenciais de serviço

+ Para simplificar, as Credenciais de serviço são lidas a partir do arquivo JSON baixado. No entanto, em cenários de uso real, as Credenciais de serviço devem ser armazenadas com segurança, de acordo com as diretrizes de segurança de sua organização

1. Gerar um JWT a partir das Credenciais de serviço
1. Trocar o JWT por um token de acesso

+ Quando as Credenciais de serviço estão presentes, o aplicativo externo usa esse token de acesso em vez do Token de acesso de desenvolvimento local, ao acessar o AEM as a Cloud Service

Neste tutorial, Adobe `@adobe/jwt-auth` O módulo npm é usado para ambos, (1) gerar o JWT a partir das Credenciais de serviço e (2) trocá-lo por um token de acesso, em uma única chamada de função. Se seu aplicativo não for baseado em JavaScript, reveja o [código de amostra em outros idiomas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) para saber como criar um JWT a partir das Credenciais de serviço e trocá-lo por um token de acesso com o Adobe IMS.

## Ler as credenciais de serviço

Revise o `getCommandLineParams()` Portanto, veja como o arquivo JSON de credenciais de serviço é lido usando o mesmo código usado para ler no JSON do token de acesso de desenvolvimento local.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## Criar um JWT e trocar por um token de acesso

Depois que as Credenciais de serviço são lidas, elas são usadas para gerar um JWT, que é então trocado com as APIs do Adobe IMS por um token de acesso. Esse token de acesso pode ser usado para acessar o AEM as a Cloud Service.

Este aplicativo de exemplo é baseado em Node.js, portanto, é melhor usá-lo [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) Módulo npm para facilitar a (1) geração de JWT e (20) troca com o Adobe IMS. Se o aplicativo for desenvolvido em outro idioma, revise [as amostras de código apropriadas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) sobre como criar a solicitação HTTP para o Adobe IMS usando outras linguagens de programação.

1. Atualize o `getAccessToken(..)` para inspecionar o conteúdo do arquivo JSON e determinar se ele representa um Token de acesso de desenvolvimento local ou Credenciais de serviço. Este objetivo pode ser facilmente alcançado verificando a `.accessToken` propriedade, que só existe para o Token de acesso de desenvolvimento local JSON.

   Se as Credenciais de serviço forem fornecidas, o aplicativo gerará um JWT e o trocará com o Adobe IMS por um token de acesso. Use o [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)do `auth(...)` Função que gera um JWT e o troca por um token de acesso em uma única chamada de função. Os parâmetros para `auth(..)` métodos são um [Objeto JSON composto de informações específicas](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponível no JSON de Credenciais de serviço, conforme descrito abaixo no código.

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    Agora, dependendo de qual arquivo JSON, o Token de acesso de desenvolvimento local JSON ou o JSON de credenciais de serviço, é transmitido por meio desse parâmetro de linha de comando &quot;file&quot;, o aplicativo derivará um token de acesso.
    
    Lembre-se, enquanto as Credenciais de serviço expiram a cada 365 dias, o JWT e o token de acesso correspondente expiram com frequência e precisam ser atualizados antes de expirarem. Isso pode ser feito usando um &quot;refresh_token&quot; [fornecido pelo Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Com essas alterações em vigor, o JSON de credenciais de serviço foi baixado do Console do desenvolvedor do AEM e, para simplificar, salvo como `service_token.json` na mesma pasta que esta `index.js`. Agora, vamos executar o aplicativo substituindo o parâmetro de linha de comando `file` com `service_token.json`e atualizando o `propertyValue` a um novo valor para que os efeitos sejam aparentes no AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   A saída para o terminal é semelhante a:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   A variável __403 - Proibido__ indica erros nas chamadas HTTP API para o AEM as a Cloud Service. Esses erros 403 Proibidos ocorrem ao tentar atualizar os metadados dos ativos.

   O motivo para isso é que o token de acesso derivado de Credenciais de serviço autentica a solicitação para o AEM usando um usuário AEM de conta técnica criado automaticamente, que, por padrão, tem somente acesso de leitura. Para fornecer ao aplicativo acesso de gravação ao AEM, o usuário da conta técnica AEM associado ao token de acesso deve receber permissão no AEM.

## Configuração do acesso no AEM

O token de acesso derivado de Credenciais de serviço usa uma conta técnica do usuário AEM que tem associação à __Colaboradores__ Grupo de usuários AEM.

![Credenciais de serviço - Conta técnica do usuário AEM](./assets/service-credentials/technical-account-user.png)

Quando o usuário técnico AEM existir no AEM (após a primeira solicitação HTTP com o token de acesso), as permissões desse usuário AEM AEM poderão ser gerenciadas da mesma forma que os outros usuários do.

1. Primeiro, localize o nome de logon AEM da conta técnica abrindo o JSON de credenciais de serviço baixado no Console do desenvolvedor AEM e localize o `integration.email` que deve ser semelhante a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Faça logon no serviço de Autor do ambiente AEM correspondente como um Administrador de AEM
1. Navegue até __Ferramentas__ > __Segurança__ > __Usuários__
1. Localize o usuário AEM com a __Nome de logon__ identificadas na Etapa 1, e abrir a sua __Propriedades__
1. Navegue até a __Grupos__ e adicione a guia __Usuários DAM__ grupo (a quem como acesso de gravação aos ativos)
1. Toque __Salvar e fechar__

Com a conta técnica permitida no AEM para ter permissões de gravação em ativos, execute o aplicativo novamente:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

A saída para o terminal é semelhante a:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificar as alterações

1. Faça logon no ambiente do AEM as a Cloud Service que foi atualizado (usando o mesmo nome de host fornecido na variável `aem` parâmetro de linha de comando)
1. Navegue até a __Assets__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pelo `folder` parâmetro de linha de comando, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Degustação de Vinho Napa__
1. Abra o __Propriedades__ para qualquer ativo na pasta
1. Navegue até a __Avançado__ guia
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que está mapeado para o atualizado `metadata/dc:rights` A propriedade JCR, que agora reflete o valor fornecido na variável `propertyValue` parâmetro, por exemplo __Uso Restrito da WKND__

![Atualização de Metadados de Uso Restrito da WKND](./assets/service-credentials/asset-metadata.png)

## Parabéns!

Agora que acessamos programaticamente o AEM as a Cloud Service usando um token de acesso de desenvolvimento local e um token de acesso de serviço para serviço pronto para produção!
