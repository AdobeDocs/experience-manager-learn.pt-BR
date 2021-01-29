---
title: Credenciais de serviço
description: AEM Credenciais de serviço são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de autor ou publicação do AEM por HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: c4f3d437b5ecfe6cb97314076cd3a5e31b184c79
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 0%

---


# Credenciais de serviço

As integrações com AEM como Cloud Service devem ser autenticadas com segurança para AEM. AEM Developer Console concede acesso às Credenciais de serviço, que são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de autor ou publicação do AEM por HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

As credenciais de serviço podem parecer [Tokens de acesso de desenvolvimento local](./local-development-access-token.md) semelhantes, mas são diferentes de algumas maneiras principais:

+ As credenciais de serviço são tokens de acesso _e não_, em vez disso, são credenciais usadas para _obter_.
+ As credenciais de serviço são mais permanentes (expiram a cada 365 dias) e não são alteradas a menos que sejam revogadas, enquanto os Tokens de acesso de Desenvolvimento local expiram diariamente.
+ As credenciais de serviço de um AEM como um ambiente mapeiam para um único usuário AEM conta técnica, enquanto os Tokens de acesso de Desenvolvimento local são autenticados como o usuário AEM que gerou o token de acesso.

As credenciais de serviço e os tokens de acesso que geram, bem como os Tokens de acesso de desenvolvimento local, devem ser mantidos em segredo, uma vez que os três podem ser usados para obter acesso aos respectivos AEM como ambientes Cloud Service

## Gerar credenciais de serviço

A geração de Credenciais de Serviço é dividida em duas etapas:

1. Uma inicialização de Credenciais de Serviço única por um administrador de Organização Adobe IMS
1. Download e uso das credenciais de serviço JSON

### Inicialização de credenciais de serviço

As credenciais de serviço, ao contrário dos Tokens de acesso de Desenvolvimento local, exigem uma _inicialização única_ pelo administrador do Adobe Org IMS antes de serem baixadas.

![Inicializar Credenciais de Serviço](assets/service-credentials/initialize-service-credentials.png)

__Esta é uma inicialização única por AEM como um ambiente Cloud Service__

1. Verifique se você está conectado como administrador da sua organização Adobe IMS
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o AEM como um ambiente Cloud Service para integrar a configuração das credenciais de serviço para
1. Toque nas reticências ao lado do ambiente na seção __Ambientes__ e selecione __Developer Console__
1. Toque na guia __Integrações__
1. Toque no botão __Obter Credenciais de Serviço__
1. As credenciais de serviço serão inicializadas e exibidas como JSON

![Console do desenvolvedor AEM - Integrações - Obter credenciais de serviço](./assets/service-credentials/developer-console.png)

Após a inicialização do AEM como credenciais de serviço de ambiente, outros desenvolvedores AEM em sua organização Adobe IMS poderão baixá-las.

### Baixar credenciais de serviço

![Baixar credenciais de serviço](assets/service-credentials/download-service-credentials.png)

Baixar as credenciais de serviço segue as mesmas etapas da inicialização. Se a inicialização ainda não tiver ocorrido, o usuário receberá um erro ao tocar no botão __Obter credenciais de serviço__.

1. Certifique-se de que você é membro do __Cloud Manager - Developer__ Perfil de produto IMS (que concede acesso ao AEM Developer Console)
   + A AEM Sandbox como ambientes Cloud Service só exige a associação no Perfil de produtos __AEM Administradores__ ou __AEM Usuários__
1. Faça logon em [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o AEM como um ambiente Cloud Service para integrar com
1. Toque nas reticências ao lado do ambiente na seção __Ambientes__ e selecione __Developer Console__
1. Toque na guia __Integrações__
1. Toque no botão __Obter Credenciais de Serviço__
1. Toque no botão de download no canto superior esquerdo para baixar o arquivo JSON que contém o valor de Credenciais de serviço e salvar o arquivo em um local seguro.
   + _Se as credenciais de serviço estiverem comprometidas, entre em contato imediatamente com o suporte à Adobe para que elas sejam revogadas_

## Instale as credenciais de serviço

As credenciais de serviço fornecem os detalhes necessários para gerar um JWT, que é trocado por um token de acesso usado para autenticação com AEM como Cloud Service. As credenciais de serviço devem ser armazenadas em um local seguro acessível aos aplicativos, sistemas ou serviços externos que as utilizam para acessar AEM. Como e onde as Credenciais de Serviço são gerenciadas serão exclusivas por cliente.

Para simplificar, este tutorial transmite as Credenciais de serviço através da linha de comando, no entanto, trabalhe com sua equipe de Segurança de TI para entender como armazenar e acessar essas credenciais de acordo com as diretrizes de segurança de sua organização.

1. Copie o [baixado o JSON de Credenciais de Serviço](#download-service-credentials) para um arquivo chamado `service_token.json` na raiz do projeto
   + Mas lembre-se, nunca confirme nada ao Git!

## Usar credenciais de serviço

As credenciais de serviço, um objeto JSON totalmente formado, não são as mesmas do JWT nem do token de acesso. Em vez disso, as credenciais de serviço (que contêm uma chave privada) são usadas para gerar um JWT, que é trocado com APIs Adobe IMS para um token de acesso.

![Credenciais de Serviço - Aplicativo Externo](assets/service-credentials/service-credentials-external-application.png)

1. Baixe as credenciais de serviço AEM Console do desenvolvedor para um local seguro
1. Um aplicativo externo precisa interagir programaticamente com AEM como ambientes Cloud Service
1. A Aplicação Externa lê nas Credenciais de Serviço a partir de um local seguro
1. O aplicativo externo usa informações das credenciais de serviço para construir um token JWT
1. O token JWT é enviado para o Adobe IMS para troca por um token de acesso
1. Adobe IMS retorna um token de acesso que pode ser usado para acessar AEM como Cloud Service
   + Tokens de acesso podem ter uma expiração solicitada. É melhor manter a vida do token de acesso curta e atualizá-la quando necessário.
1. O Aplicativo externo faz solicitações HTTP para AEM como Cloud Service, adicionando o token de acesso como um token do Portador ao cabeçalho de Autorização das solicitações HTTP
1. AEM como um Cloud Service recebe a solicitação HTTP, autentica a solicitação e executa o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP para o Aplicativo Externo

### Atualizações para o aplicativo externo

Para acessar AEM como um Cloud Service usando as credenciais de serviço, nosso aplicativo externo deve ser atualizado de três maneiras:

1. Leitura nas credenciais de serviço
   + Para simplificar, vamos lê-las no arquivo JSON baixado, no entanto, em cenários de uso real, as credenciais de serviço devem ser armazenadas com segurança, de acordo com as diretrizes de segurança de sua organização
1. Gerar um JWT a partir das credenciais de serviço
1. Troque o JWT por um token de acesso
   + Quando as Credenciais de Serviço estão presentes, nosso aplicativo externo usa esse token de acesso em vez do Token de acesso de Desenvolvimento Local, ao acessar o AEM como Cloud Service

Neste tutorial, o módulo Adobe `@adobe/jwt-auth` npm é usado para ambos, (1) gerar o JWT a partir das credenciais de serviço e (2) trocá-lo por um token de acesso, em uma única chamada de função. Se seu aplicativo não for baseado em JavaScript, reveja o código de amostra [em outros idiomas](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) para saber como criar um JWT a partir das credenciais de serviço e troque-o por um token de acesso com Adobe IMS.

## Leia as credenciais de serviço

Revise `getCommandLineParams()` e veja que podemos ler nos arquivos JSON de Credenciais de Serviço usando o mesmo código usado para ler no Token de acesso de Desenvolvimento Local JSON.

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

## Criar um JWT e trocar por um Token de acesso

Depois que as credenciais de serviço são lidas, elas são usadas para gerar um JWT que é trocado com APIs Adobe IMS para um token de acesso, que pode ser usado para acessar AEM como Cloud Service.

Este exemplo de aplicativo é baseado em Node.js, portanto, é melhor usar o módulo [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm para facilitar a geração (1) JWT e (20 troca com Adobe IMS. Se seu aplicativo for desenvolvido usando outro idioma, reveja [as amostras de código apropriadas](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) sobre como construir a solicitação HTTP para o Adobe IMS usando outras linguagens de programação.

1. Atualize `getAccessToken(..)` para inspecionar o conteúdo do arquivo JSON e determinar se ele representa um Token de acesso de desenvolvimento local ou credenciais de serviço. Isso pode ser facilmente obtido verificando a existência da propriedade `.accessToken`, que existe apenas para o Token de acesso de Desenvolvimento local JSON.

   Se as credenciais de serviço forem fornecidas, o aplicativo gerará um JWT e o trocará com o Adobe IMS para um token de acesso. Usaremos a função [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) de `auth(...)` que gera um JWT e o troca por um token de acesso em uma única chamada de função.  Os parâmetros para `auth(..)` são um objeto [JSON composto de informações específicas](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponíveis no JSON de Credenciais de Serviço, conforme descrito abaixo no código.

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

   Agora, dependendo de qual arquivo JSON - seja o Token de acesso de desenvolvimento local JSON ou o JSON de credenciais de serviço - é transmitido por meio desse parâmetro de linha de comando `file`, o aplicativo irá derivar um token de acesso.

   Lembre-se de que, embora as credenciais de serviço não expirem, o JWT e o token de acesso correspondente o fazem e precisam ser atualizados antes de expirarem. Isso pode ser feito usando um `refresh_token` [fornecido pelo Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Com essas alterações em vigor, e o JSON de Credenciais de Serviço baixado do Console do Desenvolvedor do AEM (e por questões de simplicidade, salvo como `service_token.json` a mesma pasta que este `index.js`), execute o aplicativo substituindo o parâmetro de linha de comando `file` por `service_token.json` e atualize o `propertyValue` para um novo valor, para que os efeitos fiquem visíveis no AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   A saída do terminal será semelhante a:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   As linhas __403 - Proibido__ indicam erros nas chamadas da API HTTP para AEM como Cloud Service. Esses 403 erros proibidos ocorrem ao tentar atualizar os metadados dos ativos.

   O motivo para isso é que o token de acesso derivado de Credenciais de Serviço autentica a solicitação para AEM usando uma conta técnica criada automaticamente AEM usuário, que por padrão tem somente acesso de leitura. Para fornecer ao aplicativo acesso de gravação ao AEM, a conta técnica AEM usuário associado ao token de acesso deve receber permissão em AEM.

## Configurar acesso em AEM

O token de acesso derivado de Credenciais de serviço usa uma conta técnica AEM usuário que tem associação no grupo de contribuidores AEM usuário.

![Credenciais de serviço - Conta técnica AEM usuário](./assets/service-credentials/technical-account-user.png)

Quando a conta técnica AEM usuário existir no AEM (após a primeira solicitação HTTP com o token de acesso), as permissões desse usuário AEM poderão ser gerenciadas da mesma forma que outros usuários AEM.

1. Primeiro, localize o nome de login AEM da conta técnica abrindo o JSON de Credenciais de Serviço baixado AEM Console do Desenvolvedor e localize o valor `integration.email`, que deve ser semelhante a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Faça logon no serviço Autor do ambiente correspondente como um Administrador do AEM
1. Navegue até __Ferramentas__ > __Segurança__ > __Utilizadores__
1. Localize o usuário AEM com o __Nome de login__ identificado na Etapa 1 e abra seu __Propriedades__
1. Navegue até a guia __Grupos__ e adicione o grupo __Usuários DAM__ (que gravam acesso aos ativos)
1. Toque em __Salvar e Fechar__

Com a conta técnica permitida em AEM para ter permissões de gravação em ativos, execute novamente o aplicativo:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

A saída do terminal será semelhante a:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verifique as alterações

1. Faça logon no AEM como um ambiente Cloud Service que foi atualizado (usando o mesmo nome de host fornecido no parâmetro de linha de comando `aem`)
1. Navegue até __Ativos__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pelo parâmetro de linha de comando `folder`, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Napa Vinculação__
1. Abra as __Propriedades__ para qualquer ativo na pasta
1. Navegue até a guia __Avançado__
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que está mapeada para a propriedade JCR `metadata/dc:rights` atualizada, que agora reflete o valor fornecido no parâmetro `propertyValue`, por exemplo __Uso Restrito WKND__

![Atualização de metadados de uso restrito de WKND](./assets/service-credentials/asset-metadata.png)

## Parabéns!

Agora que acessamos de forma programática AEM como um Cloud Service usando um token de acesso de desenvolvimento local, assim como um token de acesso pronto para produção!

