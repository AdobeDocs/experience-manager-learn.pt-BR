---
title: Credenciais de serviço
description: Saiba como usar as Credenciais de serviço usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação por HTTP.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
duration: 881
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1960'
ht-degree: 0%

---

# Credenciais de serviço

As integrações com o Adobe Experience Manager (AEM) as a Cloud Service devem ser capazes de autenticar com segurança no serviço do AEM. O Developer Console da AEM concede acesso às Credenciais de serviço, que são usadas para facilitar aplicativos, sistemas e serviços externos para interagir programaticamente com os serviços de Autor ou Publicação do AEM por HTTP.

O AEM integra-se com outros produtos da Adobe usando o [S2S OAuth gerenciado pela Adobe Developer Console](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/setting-up-ims-integrations-for-aem-as-a-cloud-service). Para integrações personalizadas com contas de serviço, as credenciais do JWT são usadas e gerenciadas no AEM Developer Console.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

As Credenciais de Serviço podem parecer [Tokens de Acesso de Desenvolvimento Local](./local-development-access-token.md) semelhantes, mas são diferentes de algumas maneiras principais:

+ As Credenciais de serviço são associadas às Contas técnicas. Várias credenciais de serviço podem estar ativas para uma Conta Técnica.
+ As Credenciais de Serviço são _não_ tokens de acesso, mas são credenciais usadas para _obter_ tokens de acesso.
+ As Credenciais de serviço são mais permanentes (os certificados expiram a cada 365 dias) e não são alteradas, a menos que sejam revogadas, enquanto os Tokens de acesso de desenvolvimento local expiram diariamente.
+ As Credenciais de serviço para um ambiente do AEM as a Cloud Service são mapeadas para um único usuário de conta técnica do AEM, enquanto os Tokens de acesso de desenvolvimento local são autenticados como o usuário do AEM que gerou o token de acesso.
+ Um ambiente do AEM as a Cloud Service pode ter até dez contas técnicas, cada uma com suas próprias Credenciais de serviço, cada uma mapeando para uma conta técnica distinta de usuário do AEM.

As Credenciais de serviço e os tokens de acesso gerados por elas, além dos Tokens de acesso de desenvolvimento local, devem ser mantidos em segredo. Como todos os três podem ser usados para obter, acesse o respectivo ambiente do AEM as a Cloud Service.

## Gerar credenciais de serviço

A geração de Credenciais de serviço é dividida em duas etapas:

1. Uma criação de conta técnica única por um administrador da organização Adobe IMS
1. O download e o uso das credenciais de serviço da conta técnica JSON

### Criar uma conta técnica

As Credenciais de serviço, diferentemente dos Tokens de acesso de desenvolvimento local, exigem que uma Conta técnica seja criada por um Administrador IMS da Organização Adobe antes de serem baixadas. Contas técnicas discretas devem ser criadas para cada cliente que requer acesso programático ao AEM.

![Criar uma Conta Técnica](assets/service-credentials/initialize-service-credentials.png)

As Contas técnicas são criadas uma vez, no entanto, as Chaves privadas usam o para gerenciar Credenciais de serviço associadas à Conta técnica do podem ser gerenciadas ao longo do tempo. Por exemplo, novas Credenciais de chave privada/serviço devem ser geradas antes da expiração da chave privada atual, para permitir o acesso ininterrupto por um usuário das Credenciais de serviço.

1. Verifique se você está conectado como:

   + __Administrador de sistema da Organização IMS da Adobe__
   + Membro do __Perfil de Produto IMS de Administradores do AEM__ em __AEM Author__

1. Faça logon no [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente do AEM as a Cloud Service para integrar e configurar as Credenciais de serviço para
1. Toque nas reticências ao lado do ambiente na seção __Ambientes__ e selecione __Developer Console__
1. Toque na guia __Integrações__
1. Toque na guia __Contas técnicas__
1. Toque no botão __Criar nova conta técnica__
1. As Credenciais de serviço da conta técnica são inicializadas e exibidas como JSON

![AEM Developer Console - Integrações - Obter Credenciais de Serviço](./assets/service-credentials/developer-console.png)

Depois que as Credenciais de serviço do ambiente do AEM as Cloud Service forem inicializadas, outros desenvolvedores do AEM na sua organização do Adobe IMS poderão baixá-las.

### Baixar credenciais de serviço

![Baixar Credenciais do Serviço](assets/service-credentials/download-service-credentials.png)

O download das Credenciais de serviço segue as etapas semelhantes à inicialização.

1. Verifique se você está conectado como:

   + __Administrador da organização Adobe IMS__
   + Membro do __Perfil de Produto IMS de Administradores do AEM__ em __AEM Author__

1. Faça logon no [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra o Programa que contém o ambiente do AEM as a Cloud Service para integrar com o
1. Toque nas reticências ao lado do ambiente na seção __Ambientes__ e selecione __Developer Console__
1. Toque na guia __Integrações__
1. Toque na guia __Contas técnicas__
1. Expanda a __Conta técnica__ a ser usada
1. Expanda a __Chave privada__ cujas Credenciais de Serviço serão baixadas e verifique se o status é __Ativo__
1. Toque em __...__ > __Exibir__ associado à __Chave Privada__, que exibe as Credenciais de Serviço JSON
1. Toque no botão de download no canto superior esquerdo para baixar o arquivo JSON que contém o valor Credenciais de serviço e salvar o arquivo em um local seguro

## Instalar as Credenciais de Serviço

As Credenciais de serviço fornecem os detalhes necessários para gerar um JWT, que é substituído por um token de acesso usado para autenticação com o AEM as a Cloud Service. As Credenciais de Serviço devem ser armazenadas em um local seguro acessível aos aplicativos externos, sistemas ou serviços que as utilizam para acessar o AEM. Como e onde as Credenciais de serviço são gerenciadas são exclusivas por cliente.

Para simplificar, este tutorial passa as Credenciais de serviço no pela linha de comando. No entanto, trabalhe com sua equipe de Segurança de TI para entender como armazenar e acessar essas credenciais de acordo com as diretrizes de segurança de sua organização.

1. Copie o [JSON de Credenciais de Serviço](#download-service-credentials) para um arquivo chamado `service_token.json` na raiz do projeto

   + Lembre-se, nunca confirme _nenhuma credencial_ no Git!

## Usar credenciais de serviço

As Credenciais de serviço, um objeto JSON totalmente formado, não são as mesmas que o JWT nem o token de acesso. Em vez disso, as Credenciais de serviço (que contêm uma chave privada) são usadas para gerar um JWT, que é substituído por um token de acesso pelas APIs do Adobe IMS.

![Credenciais de Serviço - Aplicativo Externo](assets/service-credentials/service-credentials-external-application.png)

1. Baixar as Credenciais de Serviço do AEM Developer Console em um local seguro
1. O aplicativo externo precisa interagir programaticamente com o ambiente do AEM as a Cloud Service
1. O Aplicativo Externo lê as Credenciais de Serviço de um local seguro
1. O aplicativo externo usa informações das credenciais de serviço para criar um token JWT
1. O token JWT é enviado ao Adobe IMS para troca por um token de acesso
1. O Adobe IMS retorna um token de acesso que pode ser usado para acessar o AEM as a Cloud Service

   + Os tokens de acesso não podem alterar um tempo de expiração.

1. O aplicativo externo faz solicitações HTTP ao AEM as a Cloud Service, adicionando o token de acesso como um token de portador ao cabeçalho de autorização das solicitações HTTP
1. O AEM as a Cloud Service recebe a solicitação HTTP, autentica a solicitação e realiza o trabalho solicitado pela solicitação HTTP e retorna uma resposta HTTP de volta ao Aplicativo externo

### Atualizações no aplicativo externo

Para acessar o AEM as a Cloud Service usando as Credenciais de serviço, o aplicativo externo deve ser atualizado de três maneiras:

1. Ler nas Credenciais de serviço

   + Para simplificar, as Credenciais de serviço são lidas a partir do arquivo JSON baixado. No entanto, em cenários de uso real, as Credenciais de serviço devem ser armazenadas com segurança, de acordo com as diretrizes de segurança de sua organização

1. Gerar um JWT a partir das Credenciais de serviço
1. Trocar o JWT por um token de acesso

   + Quando as Credenciais de serviço estão presentes, o aplicativo externo usa esse token de acesso em vez do Token de acesso de desenvolvimento local, ao acessar o AEM as a Cloud Service

Neste tutorial, o módulo `@adobe/jwt-auth` npm do Adobe é usado para ambos, (1) gerar o JWT a partir das Credenciais de serviço e (2) trocá-lo por um token de acesso, em uma única chamada de função. Se seu aplicativo não for baseado em JavaScript, você poderá desenvolver um código personalizado no idioma de sua escolha que criará o JWT a partir das Credenciais de serviço e o trocará por um token de acesso com o Adobe IMS.

## Ler as credenciais de serviço

Revise o `getCommandLineParams()` para ver como o arquivo JSON de Credenciais de Serviço é lido usando o mesmo código usado para ler no JSON do Token de Acesso de Desenvolvimento Local.

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

Este aplicativo de exemplo é baseado em Node.js, portanto, é melhor usar o módulo [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm para facilitar a (1) geração de JWT e (20) troca com o Adobe IMS. Se seu aplicativo for desenvolvido usando outra linguagem, revise [as amostras de código apropriadas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples) sobre como criar a solicitação HTTP para o Adobe IMS usando outras linguagens de programação.

1. Atualize o `getAccessToken(..)` para inspecionar o conteúdo do arquivo JSON e determinar se ele representa um Token de Acesso de Desenvolvimento Local ou Credenciais de Serviço. Isso pode ser feito facilmente verificando a existência da propriedade `.accessToken`, que só existe para o JSON do Token de Acesso de Desenvolvimento Local.

   Se as Credenciais de serviço forem fornecidas, o aplicativo gerará um JWT e o trocará com o Adobe IMS por um token de acesso. Use a função [ de ](https://www.npmjs.com/package/@adobe/jwt-auth)@adobe/jwt-auth`auth(...)` que gera um JWT e o troca por um token de acesso em uma única chamada de função. Os parâmetros para o método `auth(..)` são um [objeto JSON composto de informações específicas](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponíveis no JSON de Credenciais de Serviço, conforme descrito abaixo no código.

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

   Agora, dependendo de qual arquivo JSON - o JSON do Token de Acesso de Desenvolvimento Local ou o JSON de Credenciais de Serviço - for transmitido por meio desse parâmetro de linha de comando `file`, o aplicativo derivará um token de acesso.

   Lembre-se, enquanto as Credenciais de serviço expiram a cada 365 dias, o JWT e o token de acesso correspondente expiram com frequência e precisam ser atualizados antes de expirarem. Isso pode ser feito usando um `refresh_token` [fornecido pelo Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Com essas alterações em vigor, o JSON de Credenciais de Serviço foi baixado do AEM Developer Console e, para simplificar, salvo como `service_token.json` na mesma pasta que este `index.js`. Agora, vamos executar o aplicativo substituindo o parâmetro de linha de comando `file` por `service_token.json` e atualizando o `propertyValue` para um novo valor para que os efeitos sejam aparentes no AEM.

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

   As linhas __403 - Proibido__ indicam erros nas chamadas de API HTTP para o AEM as a Cloud Service. Esses erros 403 Proibidos ocorrem ao tentar atualizar os metadados dos ativos.

   O motivo para isso é que o token de acesso derivado de Credenciais de serviço autentica a solicitação para a AEM usando uma conta técnica criada automaticamente pelo usuário do AEM, que, por padrão, tem somente acesso de leitura. Para fornecer ao aplicativo acesso de gravação ao AEM, o usuário técnico do AEM associado ao token de acesso deve receber permissão no AEM.

## Configuração do acesso no AEM

O token de acesso derivado de Credenciais de Serviço usa uma conta técnica do Usuário do AEM que tem associação no grupo de usuários do AEM __Colaboradores__.

![Credenciais de Serviço - Usuário Técnico de Conta do AEM](./assets/service-credentials/technical-account-user.png)

Quando a conta técnica do usuário do AEM existir no AEM (após a primeira solicitação HTTP com o token de acesso), as permissões desse usuário do AEM poderão ser gerenciadas da mesma forma que os outros usuários do AEM.

1. Primeiro, localize o nome de logon AEM da conta técnica abrindo o JSON de Credenciais de Serviço baixado do AEM Developer Console e localize o valor `integration.email`, que deve ser semelhante a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Faça logon no serviço do autor do ambiente do AEM correspondente como um administrador do AEM
1. Navegue até __Ferramentas__ > __Segurança__ > __Usuários__
1. Localize o usuário do AEM com o __Nome de Logon__ identificado na Etapa 1 e abra suas __Propriedades__
1. Navegue até a guia __Grupos__ e adicione o grupo __Usuários DAM__ (que têm acesso de gravação aos ativos)

   + [Consulte a lista de grupos de usuários fornecidos pela AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html#built-in-users-and-groups) para adicionar o usuário do serviço para obter as permissões ideais. Se nenhum grupo de usuários fornecido pela AEM for suficiente, crie o seu próprio grupo e adicione as permissões apropriadas.

1. Toque em __Salvar e fechar__

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

1. Faça logon no ambiente AEM as a Cloud Service que foi atualizado (usando o mesmo nome de host fornecido no parâmetro de linha de comando `aem`)
1. Navegue até __Assets__ > __Arquivos__
1. Navegue até a pasta de ativos especificada pelo parâmetro de linha de comando `folder`, por exemplo __WKND__ > __Inglês__ > __Aventuras__ > __Napa Wine Tasting__
1. Abra as __Propriedades__ de qualquer ativo na pasta
1. Navegue até a guia __Avançado__
1. Revise o valor da propriedade atualizada, por exemplo __Copyright__ que está mapeada para a propriedade JCR `metadata/dc:rights` atualizada, que agora reflete o valor fornecido no parâmetro `propertyValue`, por exemplo __Uso Restrito de WKND__

![Atualização de Metadados de Uso Restrito do WKND](./assets/service-credentials/asset-metadata.png)

## Parabéns!

Agora que acessamos programaticamente o AEM as a Cloud Service usando um token de acesso de desenvolvimento local e um token de acesso de serviço para serviço pronto para produção!
