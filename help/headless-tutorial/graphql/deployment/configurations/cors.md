---
title: Configuração do CORS para AEM GraphQL
description: Saiba como configurar o CORS (Cross-origin resource sharing, compartilhamento de recursos entre origens) para usar com o AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 185
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---

# Compartilhamento de recursos entre origens (CORS)

O Compartilhamento de recursos entre origens (CORS) da Adobe Experience Manager as a Cloud Service facilita que propriedades da Web que não sejam da AEM façam chamadas do lado do cliente baseadas em navegador para as APIs GraphQL da AEM e outros recursos headless da AEM.

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para se alinharem aos requisitos do seu projeto.

## Requisito do CORS

O CORS é necessário para conexões baseadas em navegador com as APIs do AEM GraphQL, quando o cliente que se conecta ao AEM NÃO é distribuído a partir da mesma origem (também conhecida como host ou domínio) do AEM.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente da Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requer a configuração do CORS | ✔ | ✔ | ✘ | ✘ |

## Autor do AEM

A ativação do CORS no serviço do AEM Author é diferente dos serviços do AEM Publish e AEM Preview. O serviço do AEM Author requer que uma configuração OSGi seja adicionada à pasta de modo de execução do serviço do AEM Author e não usa uma configuração do Dispatcher.

### Configuração OSGi

A fábrica de configuração OSGi do AEM CORS define os critérios de permissão para aceitar solicitações HTTP CORS.

| O cliente se conecta ao | Autor do AEM | Publicação no AEM | Visualização do AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração OSGi do CORS | ✔ | ✘ | ✘ |


O exemplo abaixo define uma configuração OSGi para o AEM Author (`../config.author/..`), de modo que ele só esteja ativo no serviço do AEM Author.

As principais propriedades de configuração são:

+ `alloworigin` e/ou `alloworiginregexp` especifica as origens em que o cliente que se conecta à AEM Web é executado.
+ `allowedpaths` especifica os padrões de caminho de URL permitidos nas origens especificadas.
   + Para dar suporte a consultas persistentes do AEM GraphQL, adicione o seguinte padrão: `/graphql/execute.json.*`
   + Para suportar Fragmentos de experiência, adicione o seguinte padrão: `/content/experience-fragments/.*`
+ `supportedmethods` especifica os métodos HTTP permitidos para as solicitações CORS. Para dar suporte a consultas persistentes (e Fragmentos de experiência) do AEM GraphQL, adicione `GET` .
+ `supportedheaders` inclui `"Authorization"`, pois as solicitações ao AEM Author devem ser autorizadas.
+ `supportscredentials` está definido como `true`, pois a solicitação ao AEM Author deve ser autorizada.

[Saiba mais sobre a configuração OSGi do CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

O exemplo a seguir suporta o uso de consultas persistentes do AEM GraphQL no AEM Author. Para usar consultas GraphQL definidas pelo cliente, adicione uma URL de ponto de extremidade GraphQL em `allowedpaths` e `POST` a `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### Exemplo de configuração OSGi

+ [Um exemplo da configuração OSGi pode ser encontrado no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Publicação no AEM

A ativação do CORS nos serviços de publicação (e pré-visualização) do AEM é diferente do serviço do AEM Author. O serviço de Publicação do AEM exige que uma configuração do AEM Dispatcher seja adicionada à configuração do Dispatcher do AEM Publish. A Publicação do AEM não usa uma [configuração OSGi](#osgi-configuration).

Ao configurar o CORS na publicação do AEM, verifique:

+ O cabeçalho da solicitação HTTP `Origin` não pode ser enviado para o serviço de Publicação do AEM, removendo o cabeçalho `Origin` (se adicionado anteriormente) do arquivo `clientheaders.any` do projeto do AEM Dispatcher. Todos os cabeçalhos `Access-Control-` devem ser removidos do arquivo `clientheaders.any`, e o Dispatcher os gerencia, não o serviço de Publicação do AEM.
+ Se você tiver [configurações OSGi do CORS](#osgi-configuration) habilitadas em seu serviço de Publicação do AEM, será necessário removê-las e migrar suas configurações para a [configuração do Dispatcher vhost](#set-cors-headers-in-vhost) descrita abaixo.

### Configuração do Dispatcher

O Dispatcher do serviço de Publicação (e Visualização) do AEM deve ser configurado para ser compatível com o CORS.

| O cliente se conecta ao | Autor do AEM | Publicação no AEM | Visualização do AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração do Dispatcher CORS | ✘ | ✔ | ✔ |

#### Definir cabeçalhos CORS no vhost

1. Abra o arquivo de configuração do vhost para o serviço de Publicação do AEM, em seu projeto de configuração do Dispatcher, normalmente em `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copie o conteúdo do bloco `<IfDefine ENABLE_CORS>...</IfDefine>` abaixo para o arquivo de configuração do vhost habilitado.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Faça a correspondência das Origens desejadas que acessam seu serviço de Publicação do AEM, atualizando a expressão regular na linha abaixo. Se várias origens forem necessárias, duplique esta linha e atualize para cada origem/padrão de origem.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + Por exemplo, para habilitar o acesso ao CORS a partir das origens:

      + Qualquer subdomínio em `https://example.com`
      + Qualquer porta em `http://localhost`

     Substitua a linha pelas duas linhas a seguir:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Exemplo de configuração do Dispatcher

+ [Um exemplo de configuração do Dispatcher pode ser encontrado no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
