---
title: Credenciais de Serviço
description: As Credenciais do serviço do AEM são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Integrações headless
role: Desenvolvedor
level: Intermediário, Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# Credenciais de Serviço

As integrações com o AEM as a Cloud Service devem ser capazes de autenticar com segurança no AEM. O Console do desenvolvedor do AEM concede acesso às Credenciais de serviço, que são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

As Credenciais de Serviço podem parecer semelhantes [Tokens de Acesso ao Desenvolvimento Local](./local-development-access-token.md) mas são diferentes de algumas maneiras principais:

+ As Credenciais de Serviço são _e não_ tokens de acesso, em vez de serem credenciais usadas para _obter_ tokens de acesso.
+ As credenciais de serviço são mais permanentes (expiram a cada 365 dias) e não são alteradas a menos que sejam revogadas, enquanto os tokens de acesso de desenvolvimento local expiram diariamente.
+ As credenciais de serviço de um ambiente do AEM as a Cloud Service são mapeadas para um único usuário de conta técnica do AEM, enquanto os tokens de acesso de desenvolvimento local são autenticados como o usuário do AEM que gerou o token de acesso.

As Credenciais de serviço e os tokens de acesso que geram, bem como os Tokens de Acesso de Desenvolvimento Local devem ser mantidos secretos, pois todos os três podem ser usados para obter acesso aos respectivos ambientes do AEM as a Cloud Service

## Gerar Credenciais de Serviço

A geração de Credenciais de Serviço é dividida em duas etapas:

1. Uma inicialização única das Credenciais de serviço por um administrador da Org do Adobe IMS
1. O download e o uso do JSON de credenciais de serviço

### Inicialização de Credenciais de Serviço

As credenciais de serviço, ao contrário dos tokens de acesso de desenvolvimento local, exigem uma _inicialização única_ pelo administrador do Adobe Org IMS antes que possam ser baixadas.

![Inicializar Credenciais de Serviço](assets/service-credentials/initialize-service-credentials.png)

__Esta é uma inicialização única por ambiente do AEM as a Cloud Service__

1. Verifique se você está conectado como o administrador da organização do Adobe IMS
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente do AEM as a Cloud Service para integrar a configuração das Credenciais de serviço para
1. Toque nas reticências ao lado do ambiente na seção __Ambientes__ e selecione __Console do Desenvolvedor__
1. Toque na guia __Integrações__
1. Toque no botão __Obter Credenciais de Serviço__
1. As Credenciais de Serviço serão inicializadas e exibidas como JSON

![Console do desenvolvedor do AEM - Integrações - Obter credenciais de serviço](./assets/service-credentials/developer-console.png)

Depois que as credenciais de serviço do ambiente do AEM as Cloud Service forem inicializadas, outros desenvolvedores do AEM na sua organização do Adobe IMS poderão baixá-las.

### Baixar credenciais do serviço

![Baixar credenciais do serviço](assets/service-credentials/download-service-credentials.png)

Baixar as credenciais de serviço segue as mesmas etapas da inicialização. Se a inicialização ainda não tiver ocorrido, o usuário receberá um erro ao tocar no botão __Obter credenciais de serviço__.

1. Certifique-se de que você é membro do __Cloud Manager - Developer__ Perfil de produto IMS (que concede acesso ao Console do desenvolvedor do AEM)
   + Os ambientes do Sandbox AEM as a Cloud Service exigem associação somente nos __Administradores do AEM__ ou __Usuários do AEM__ Perfil de produto
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente do AEM as a Cloud Service para integrar o
1. Toque nas reticências ao lado do ambiente na seção __Ambientes__ e selecione __Console do Desenvolvedor__
1. Toque na guia __Integrações__
1. Toque no botão __Obter Credenciais de Serviço__
1. Toque no botão de download no canto superior esquerdo para baixar o arquivo JSON que contém o valor de Credenciais de serviço e salvar o arquivo em um local seguro.
   + _Se as credenciais de serviço estiverem comprometidas, entre em contato imediatamente com o Suporte da Adobe para que sejam revogadas_

## Instalar as credenciais de serviço

As Credenciais de serviço fornecem os detalhes necessários para gerar uma JWT, que é trocada por um token de acesso usado para autenticar com o AEM as a Cloud Service. As Credenciais de serviço devem ser armazenadas em um local seguro acessível pelos aplicativos, sistemas ou serviços externos que as usam para acessar o AEM. Como e onde as credenciais de serviço são gerenciadas serão exclusivas por cliente.

Para simplificar, este tutorial transmite as Credenciais de serviço no por meio da linha de comando. No entanto, trabalhe com a equipe de Segurança de TI para entender como armazenar e acessar essas credenciais de acordo com as diretrizes de segurança de sua organização.

1. Copie o [baixado o JSON de credenciais de serviço](#download-service-credentials) para um arquivo chamado `service_token.json` na raiz do projeto
   + Mas lembre-se, nunca confira nenhuma credencial ao Git!

## Usar Credenciais de Serviço

As Credenciais de serviço, um objeto JSON totalmente formado, não são as mesmas do JWT nem do token de acesso. Em vez disso, as Credenciais de serviço (que contêm uma chave privada) são usadas para gerar um JWT, que é trocado por APIs do Adobe IMS para um token de acesso.

![Credenciais de Serviço - Aplicativo Externo](assets/service-credentials/service-credentials-external-application.png)

1. Baixe as credenciais de serviço do Console do desenvolvedor do AEM para um local seguro
1. Um aplicativo externo precisa interagir programaticamente com os ambientes do AEM as a Cloud Service
1. O Aplicativo Externo lê nas Credenciais de Serviço a partir de um local seguro
1. O Aplicativo Externo usa informações das Credenciais de Serviço para construir um Token JWT
1. O token JWT é enviado ao Adobe IMS para troca por um token de acesso
1. O Adobe IMS retorna um token de acesso que pode ser usado para acessar o AEM as a Cloud Service
   + Os tokens de acesso podem ter uma expiração solicitada. É melhor manter a vida do token de acesso curta e atualizar quando necessário.
1. O aplicativo externo faz solicitações HTTP para o AEM as a Cloud Service, adicionando o token de acesso como um token portador ao cabeçalho de Autorização das solicitações HTTP
1. O AEM as a Cloud Service recebe a solicitação HTTP, autentica a solicitação e executa o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP para o aplicativo externo

### Atualizações para o aplicativo externo

Para acessar o AEM as a Cloud Service usando as credenciais de serviço, nosso aplicativo externo deve ser atualizado de 3 maneiras:

1. Lido nas Credenciais de Serviço
   + Para simplificar, leremos esses itens do arquivo JSON baixado. No entanto, em cenários de uso real, as Credenciais de serviço devem ser armazenadas com segurança de acordo com as diretrizes de segurança de sua organização
1. Gerar um JWT a partir das credenciais de serviço
1. Troque o JWT por um token de acesso
   + Quando as Credenciais de serviço estão presentes, nosso aplicativo externo usa esse token de acesso em vez do Token de acesso de desenvolvimento local, ao acessar o AEM as a Cloud Service

Neste tutorial, o módulo `@adobe/jwt-auth` npm da Adobe é usado para ambos, (1) gerar o JWT a partir das Credenciais de serviço e (2) trocá-lo por um token de acesso, em uma única chamada de função. Se o aplicativo não for baseado em JavaScript, revise o [código de amostra em outros idiomas](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) para saber como criar um JWT a partir das credenciais de serviço e troque-o por um token de acesso com o Adobe IMS.

## Leia as credenciais de serviço

Revise o `getCommandLineParams()` e veja que podemos ler nos arquivos JSON de Credenciais de Serviço usando o mesmo código usado para ler no JSON do Token de Acesso de Desenvolvimento Local.

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

Depois que as Credenciais de serviço são lidas, elas são usadas para gerar uma JWT que é trocada com as APIs do Adobe IMS por um token de acesso, que pode ser usado para acessar o AEM as a Cloud Service.

Este aplicativo de exemplo é baseado em Node.js, portanto, é melhor usar o módulo [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm para facilitar a (1) geração de JWT e (20 troca com o Adobe IMS. Se seu aplicativo for desenvolvido usando outro idioma, revise [as amostras de código apropriadas](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) sobre como construir a solicitação HTTP para o Adobe IMS usando outras linguagens de programação.

1. Atualize o `getAccessToken(..)` para inspecionar o conteúdo do arquivo JSON e determinar se ele representa um Token de Acesso de Desenvolvimento Local ou Credenciais de Serviço. Isso pode ser feito facilmente verificando a existência da propriedade `.accessToken`, que só existe para o Token de Acesso ao Desenvolvimento Local JSON.

   Se as Credenciais de Serviço forem fornecidas, o aplicativo gerará um JWT e o intercambiará com o Adobe IMS para um token de acesso. Usaremos a função [ &lt;jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) de `auth(...)` que gera um JWT e o troca para um token de acesso em uma única chamada de função.  Os parâmetros para `auth(..)` são um [JSON objeto composto de informações específicas](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponíveis no JSON de credenciais de serviço, conforme descrito abaixo no código.

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

   Agora, dependendo de qual arquivo JSON - o JSON do Token de Acesso ao Desenvolvimento Local ou o JSON de Credenciais do Serviço - é passado por meio desse parâmetro de linha de comando `file`, o aplicativo vai derivar um token de acesso.

   Lembre-se de que, embora as Credenciais de Serviço não expirem, o JWT e o token de acesso correspondente o fazem e precisam ser atualizados antes de expirarem. Isso pode ser feito usando um `refresh_token` [fornecido pelo Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Com essas alterações em vigor e o JSON de Credenciais de Serviço baixado do Console do Desenvolvedor do AEM (e para simplificar, salvo como `service_token.json` na mesma pasta que este `index.js`), execute o aplicativo substituindo o parâmetro de linha de comando `file` por `service_token.json` e atualize o `propertyValue` para um novo valor para que os efeitos sejam aparentes no AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   A saída para o terminal terá a seguinte aparência:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   As linhas __403 - Proibido__ indicam erros nas chamadas da API HTTP para o AEM as a Cloud Service. Esses erros 403 Forbidden ocorrem ao tentar atualizar os metadados dos ativos.

   O motivo para isso é que o token de acesso derivado das Credenciais de serviço autentica a solicitação para o AEM usando uma conta técnica criada automaticamente para o AEM usuário, que por padrão só tem acesso de leitura. Para fornecer o acesso de gravação do aplicativo ao AEM, o usuário da conta técnica do AEM associado ao token de acesso deve receber permissão no AEM.

## Configuração do acesso no AEM

O token de acesso derivado das Credenciais de serviço usa uma conta técnica Usuário do AEM que tem associação no grupo de usuários do AEM Contribuidores .

![Credenciais de serviço - Conta técnica do usuário do AEM](./assets/service-credentials/technical-account-user.png)

Quando a conta técnica do usuário do AEM existir no AEM (após a primeira solicitação HTTP com o token de acesso), as permissões desse usuário do AEM poderão ser gerenciadas da mesma forma que outros usuários do AEM.

1. Primeiro, localize o nome de logon da conta técnica no AEM abrindo o JSON de credenciais de serviço baixado do Console do desenvolvedor do AEM e localize o valor `integration.email`, que deve ser semelhante a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Faça logon no serviço Autor do ambiente AEM correspondente como um Administrador do AEM
1. Navegue até __Ferramentas__ > __Segurança__ > __Usuários__
1. Localize o usuário do AEM com o __Nome de logon__ identificado na Etapa 1 e abra suas __Propriedades__
1. Navegue até a guia __Grupos__ e adicione o grupo __Usuários do DAM__ (que tem acesso de gravação a ativos)
1. Toque em __Salvar e fechar__

Com a conta técnica permitida no AEM para ter permissões de gravação em ativos, execute o aplicativo novamente:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

A saída para o terminal terá a seguinte aparência:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificar as alterações

1. Faça logon no ambiente do AEM as a Cloud Service que foi atualizado (usando o mesmo nome de host fornecido no parâmetro de linha de comando `aem` )
1. Navegue até __Assets__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pelo parâmetro de linha de comando `folder`, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Napa Acondicionamento de Vinho__
1. Abra as __Propriedades__ para qualquer ativo na pasta
1. Navegue até a guia __Avançado__
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que está mapeado para a propriedade JCR `metadata/dc:rights` atualizada, que agora reflete o valor fornecido no parâmetro `propertyValue`, por exemplo __WKND Restricted Use__

![Atualização de metadados de uso restrito de WKND](./assets/service-credentials/asset-metadata.png)

## Parabéns!

Agora que acessamos programaticamente o AEM as a Cloud Service usando um token de acesso de desenvolvimento local, bem como um token de acesso de serviço para serviço pronto para produção!

