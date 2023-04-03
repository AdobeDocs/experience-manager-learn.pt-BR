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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1937'
ht-degree: 0%

---

# Credenciais de serviço

As integrações com o Adobe Experience Manager (AEM) as a Cloud Service devem ser capazes de autenticar com segurança para AEM serviço. O Console do desenvolvedor AEM concede acesso às Credenciais de serviço, que são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

As credenciais de serviço podem ser semelhantes [Tokens de Acesso ao Desenvolvimento Local](./local-development-access-token.md) mas são diferentes de algumas maneiras principais:

+ As Credenciais de Serviço estão associadas às Contas Técnicas. Várias credenciais de serviço podem estar ativas para uma Conta Técnica.
+ As credenciais de serviço são _not_ tokens de acesso, em vez de serem credenciais usadas para _obter_ acessar tokens.
+ As Credenciais de Serviço são mais permanentes (seus certificados expiram a cada 365 dias) e não são alteradas a menos que sejam revogadas, enquanto os Tokens de Acesso ao Desenvolvimento Local expiram diariamente.
+ As Credenciais de Serviço para um ambiente AEM as a Cloud Service mapeiam para um único usuário de conta técnica AEM, enquanto os Tokens de Acesso ao Desenvolvimento Local são autenticados como o usuário AEM que gerou o token de acesso.
+ Um ambiente AEM as a Cloud Service pode ter até dez contas técnicas, cada uma com suas próprias Credenciais de Serviço, cada mapeamento para conta técnica discreta AEM usuário.

As Credenciais de Serviço e os tokens de acesso gerados por elas, bem como os Tokens de Acesso ao Desenvolvimento Local, devem ser mantidas secretas. Como todos os três podem ser usados para obter acesso ao respectivo ambiente AEM as a Cloud Service.

## Gerar Credenciais de Serviço

A geração de Credenciais de Serviço é dividida em duas etapas:

1. Uma criação única de conta técnica por um administrador de organização do Adobe IMS
1. O download e o uso do JSON de credenciais de serviço da conta técnica

### Criar uma conta técnica

As credenciais de serviço, ao contrário dos tokens de acesso de desenvolvimento local, exigem que uma conta técnica seja criada por um administrador IMS da organização do Adobe para que possa ser baixada. Devem ser criadas contas técnicas distintas para cada cliente que exija acesso programático a AEM.

![Criar uma conta técnica](assets/service-credentials/initialize-service-credentials.png)

As contas técnicas são criadas uma vez, no entanto, as chaves privadas usam para gerenciar as credenciais de serviço associadas à conta técnica que podem ser gerenciadas ao longo do tempo. Por exemplo, novas Credenciais de Chave Privada/Serviço devem ser geradas antes da expiração da Chave Privada atual, para permitir o acesso ininterrupto por um usuário das Credenciais de Serviço.

1. Verifique se você está conectado como um:
   + __Administrador da Org do Adobe IMS__
   + Membro da __Administradores de AEM__ Perfil de produto IMS em __Autor do AEM__
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente as a Cloud Service AEM para integrar a configuração das Credenciais de Serviço para
1. Toque nas reticências ao lado do ambiente no __Ambientes__ e selecione __Console do desenvolvedor__
1. Toque em __Integrações__ guia
1. Toque no __Contas técnicas__ guia
1. Toque __Criar nova conta técnica__ botão
1. As Credenciais de Serviço da Conta Técnica são inicializadas e exibidas como JSON

![Console do desenvolvedor do AEM - Integrações - Obter credenciais de serviço](./assets/service-credentials/developer-console.png)

Depois que as AEM como credenciais de serviço do ambiente do Cloud Service forem inicializadas, outros desenvolvedores de AEM na sua Org do Adobe IMS poderão baixá-las.

### Baixar credenciais do serviço

![Baixar credenciais do serviço](assets/service-credentials/download-service-credentials.png)

Baixar as credenciais de serviço segue as etapas semelhantes da inicialização.

1. Verifique se você está conectado como um:
   + __Administrador da Org do Adobe IMS__
   + Membro da __Administradores de AEM__ Perfil de produto IMS em __Autor do AEM__
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente as a Cloud Service AEM para integrar o
1. Toque nas reticências ao lado do ambiente no __Ambientes__ e selecione __Console do desenvolvedor__
1. Toque em __Integrações__ guia
1. Toque no __Contas técnicas__ guia
1. Expanda o __Conta técnica__ a utilizar
1. Expanda o __Chave de privacidade__ cujas Credenciais de Serviço serão baixadas e verifique se o status é __Ativo__
1. Toque em __...__ > __Exibir__ associada à __Chave de privacidade__, que exibe o JSON de credenciais de serviço
1. Toque no botão de download no canto superior esquerdo para baixar o arquivo JSON que contém o valor de Credenciais de serviço e salvar o arquivo em um local seguro

## Instalar as credenciais de serviço

As Credenciais de Serviço fornecem os detalhes necessários para gerar um JWT, que é trocado por um token de acesso usado para autenticação com AEM as a Cloud Service. As Credenciais de Serviço devem ser armazenadas em um local seguro acessível pelos aplicativos, sistemas ou serviços externos que as usam para acessar o AEM. Como e onde as credenciais de serviço são gerenciadas são exclusivas por cliente.

Para simplificar, este tutorial transmite as Credenciais de serviço no por meio da linha de comando. No entanto, trabalhe com a equipe de segurança de TI para entender como armazenar e acessar essas credenciais de acordo com as diretrizes de segurança de sua organização.

1. Copie o [baixado o JSON de credenciais de serviço](#download-service-credentials) para um arquivo nomeado `service_token.json` na raiz do projeto
   + Lembre-se, nunca cometer _quaisquer credenciais_ para Git!

## Usar Credenciais de Serviço

As Credenciais de serviço, um objeto JSON totalmente formado, não são as mesmas do JWT nem do token de acesso. Em vez disso, as Credenciais de serviço (que contêm uma chave privada) são usadas para gerar um JWT, que é trocado por APIs do Adobe IMS para um token de acesso.

![Credenciais de Serviço - Aplicativo Externo](assets/service-credentials/service-credentials-external-application.png)

1. Baixe as credenciais de serviço AEM Console do desenvolvedor para um local seguro
1. O aplicativo externo precisa interagir programaticamente com AEM ambiente as a Cloud Service
1. O Aplicativo Externo lê nas Credenciais de Serviço a partir de um local seguro
1. O Aplicativo Externo usa informações das Credenciais de Serviço para construir um Token JWT
1. O token JWT é enviado ao Adobe IMS para troca por um token de acesso
1. O Adobe IMS retorna um token de acesso que pode ser usado para acessar AEM as a Cloud Service
   + Os tokens de acesso podem ter uma expiração solicitada. É melhor manter a vida do token de acesso curta e atualizar quando necessário.
1. O Aplicativo Externo faz solicitações HTTP para AEM as a Cloud Service, adicionando o token de acesso como um token portador ao cabeçalho de Autorização das solicitações HTTP
1. AEM as a Cloud Service recebe a solicitação HTTP, autentica a solicitação e executa o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP ao Aplicativo Externo

### Atualizações para o aplicativo externo

Para acessar AEM as a Cloud Service usando as Credenciais de Serviço, o aplicativo externo deve ser atualizado de três maneiras:

1. Lido nas Credenciais de Serviço

+ Para simplificar, as Credenciais de Serviço são lidas a partir do arquivo JSON baixado. No entanto, em cenários de uso real, as Credenciais de Serviço devem ser armazenadas com segurança de acordo com as diretrizes de segurança de sua organização

1. Gerar um JWT a partir das credenciais de serviço
1. Troque o JWT por um token de acesso

+ Quando as Credenciais de Serviço estão presentes, o aplicativo externo usa esse token de acesso em vez do Token de Acesso de Desenvolvimento Local, ao acessar AEM as a Cloud Service

Neste tutorial, Adobe `@adobe/jwt-auth` o módulo npm é usado para ambos, (1) gerar o JWT a partir das Credenciais de Serviço e (2) trocá-lo por um token de acesso, em uma única chamada de função. Se o aplicativo não for baseado em JavaScript, revise a [código de amostra em outros idiomas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) para criar um JWT a partir das credenciais de serviço e trocá-lo por um token de acesso com o Adobe IMS.

## Leia as credenciais de serviço

Revise o `getCommandLineParams()` veja como o arquivo JSON de credenciais de serviço é lido usando o mesmo código usado para ler no JSON do token de acesso de desenvolvimento local.

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

Depois que as Credenciais de serviço são lidas, elas são usadas para gerar um JWT que é trocado com as APIs do Adobe IMS por um token de acesso. Esse token de acesso pode ser usado para acessar AEM as a Cloud Service.

Este aplicativo de exemplo é baseado em Node.js, portanto, é melhor usar [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) módulo npm para facilitar a (1) geração de JWT e (20 troca com o Adobe IMS. Se o seu aplicativo for desenvolvido usando outra língua, reveja [as amostras de código adequadas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) sobre como criar a solicitação HTTP para o Adobe IMS usando outras linguagens de programação.

1. Atualize o `getAccessToken(..)` para inspecionar o conteúdo do arquivo JSON e determinar se ele representa um token de acesso de desenvolvimento local ou credenciais de serviço. Isso pode ser facilmente feito verificando a existência da variável `.accessToken` , que só existe para o Token de Acesso ao Desenvolvimento Local JSON.

   Se as Credenciais de Serviço forem fornecidas, o aplicativo gerará um JWT e o intercambiará com o Adobe IMS para um token de acesso. Use o [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)&#39;s `auth(...)` que gera um JWT e o troca por um token de acesso em uma única chamada de função. Os parâmetros para `auth(..)` são um método [Objeto JSON composto de informações específicas](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponível no JSON de credenciais de serviço, conforme descrito abaixo no código.

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

    Agora, dependendo de qual arquivo JSON - o JSON do Token de Acesso ao Desenvolvimento Local ou o JSON de Credenciais do Serviço - é passado por meio desse parâmetro de linha de comando &quot;arquivo&quot;, o aplicativo vai derivar um token de acesso.
    
    Lembre-se de que, enquanto as Credenciais de Serviço expiram a cada 365 dias, o JWT e o token de acesso correspondente expiram com frequência e precisam ser atualizados antes de expirarem. Isso pode ser feito usando um &quot;refresh_token&quot; [fornecido pelo Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Com essas alterações em vigor, o JSON de credenciais de serviço foi baixado do Console do desenvolvedor do AEM e para simplificar, salvo como `service_token.json` na mesma pasta que esta `index.js`. Agora, vamos executar o aplicativo substituindo o parâmetro da linha de comando `file` com `service_token.json`e a atualização do `propertyValue` para um novo valor, de modo que os efeitos sejam aparentes em AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   A saída para o terminal tem a seguinte aparência:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   O __403 - Proibido__ , indique erros nas chamadas da API HTTP para AEM as a Cloud Service. Esses erros 403 Forbidden ocorrem ao tentar atualizar os metadados dos ativos.

   O motivo para isso é que o token de acesso derivado das Credenciais de Serviço autentica a solicitação para AEM usando uma conta técnica AEM usuário criada automaticamente, que por padrão tem apenas acesso de leitura. Para fornecer ao aplicativo acesso de gravação ao AEM, a conta técnica AEM usuário associada ao token de acesso deve receber permissão em AEM.

## Configurar o acesso no AEM

O token de acesso derivado das Credenciais de Serviço usa uma conta técnica AEM Usuário que tem associação no __Contribuidores__ AEM grupo de usuários.

![Credenciais de Serviço - Conta Técnica AEM Usuário](./assets/service-credentials/technical-account-user.png)

Quando a conta técnica AEM usuário existir no AEM (após a primeira solicitação HTTP com o token de acesso), essas permissões AEM usuário poderão ser gerenciadas da mesma forma que outros usuários AEM.

1. Primeiro, localize o nome de logon AEM da conta técnica abrindo o JSON de Credenciais de Serviço baixado AEM Console do Desenvolvedor e localize o `integration.email` , que deve ser semelhante a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Faça logon no serviço Autor do ambiente AEM correspondente como um Administrador AEM
1. Navegar para __Ferramentas__ > __Segurança__ > __Usuários__
1. Localize o usuário AEM com a variável __Nome de logon__ identificado na Etapa 1 e abra seu __Propriedades__
1. Navegue até o __Grupos__ e adicione a __Usuários de DAM__ grupo (que como acesso de gravação a ativos)
1. Toque __Salvar e fechar__

Com a conta técnica permitida no AEM para ter permissões de gravação em ativos, execute novamente o aplicativo:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

A saída para o terminal tem a seguinte aparência:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificar as alterações

1. Faça logon no ambiente as a Cloud Service AEM que foi atualizado (usando o mesmo nome de host fornecido no `aem` parâmetro da linha de comando)
1. Navegue até o __Ativos__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pela `folder` parâmetro da linha de comando, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Brinde de Vinho Napa__
1. Abra o __Propriedades__ para qualquer ativo na pasta
1. Navegue até o __Avançado__ guia
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que é mapeado para o `metadata/dc:rights` Propriedade JCR, que agora reflete o valor fornecido no `propertyValue` , por exemplo __Uso restrito de WKND__

![Atualização de metadados de uso restrito de WKND](./assets/service-credentials/asset-metadata.png)

## Parabéns!

Agora que acessamos de forma programática AEM as a Cloud Service usando um token de acesso de desenvolvimento local e um token de acesso de serviço para serviço pronto para produção!
