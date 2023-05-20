---
title: Configuração do CORS para AEM GraphQL
description: Saiba como configurar o Compartilhamento de recursos entre origens (CORS) para uso com o AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# Compartilhamento de recursos entre origens (CORS)

O Compartilhamento de recursos entre origens (CORS) da Adobe Experience Manager as a Cloud Service facilita que propriedades da Web que não sejam de AEM façam chamadas do lado do cliente baseadas em navegador para APIs do GraphQL AEM.

O artigo a seguir descreve como configurar _origem única_ acesso a um conjunto específico de endpoints do AEM Headless por meio do CORS. Origem única significa apenas acessos de domínio não-AEM únicos AEM, por exemplo, https://app.example.com que se conecta a https://www.example.com. O acesso de várias origens pode não funcionar usando essa abordagem devido a preocupações com armazenamento em cache.

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para se alinharem aos requisitos do seu projeto.

## Requisito do CORS

O CORS é necessário para conexões baseadas em navegador com APIs do AEM GraphQL, quando o cliente que se conecta ao AEM AEM NÃO é atendido a partir da mesma origem (também conhecida como host ou domínio) que o.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requer a configuração do CORS | ✔ | ✔ | ✘ | ✘ |

## Configuração OSGi

A fábrica de configuração OSGi CORS do AEM define os critérios de permissão para aceitar solicitações HTTP CORS.

| O cliente se conecta ao | Autor do AEM | AEM Publish | Visualização do AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração OSGi do CORS | ✔ | ✔ | ✔ |


O exemplo abaixo define uma configuração OSGi para publicação no AEM (`../config.publish/..`), mas pode ser adicionado a [qualquer pasta de modo de execução compatível](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

As principais propriedades de configuração são:

+ `alloworigin` e/ou `alloworiginregexp` especifica as origens em que o cliente que se conecta à web AEM é executado.
+ `allowedpaths` especifica os padrões de caminho de URL permitidos nas origens especificadas.
   + Para dar suporte a consultas persistentes do AEM GraphQL, adicione o seguinte padrão: `/graphql/execute.json.*`
   + Para oferecer suporte a Fragmentos de experiência, adicione o seguinte padrão: `/content/experience-fragments/.*`
+ `supportedmethods` especifica os métodos HTTP permitidos para as solicitações do CORS. Adicionar `GET`, para suportar consultas persistentes do AEM GraphQL (e Fragmentos de experiência).

[Saiba mais sobre a configuração OSGi do CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

Este exemplo de configuração suporta o uso de consultas persistentes do AEM GraphQL. Para usar consultas do GraphQL definidas pelo cliente, adicione um URL de endpoint do GraphQL em `allowedpaths` e `POST` para `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

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
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Solicitações autorizadas da API do GraphQL para AEM

Ao acessar APIs do GraphQL com AEM que exigem autorização (normalmente um autor do AEM ou conteúdo protegido no AEM Publish), verifique se a configuração OSGi do CORS tem os valores adicionais:

+ `supportedheaders` também lista `"Authorization"`
+ `supportscredentials` está definida como `true`

As solicitações autorizadas para APIs do GraphQL com AEM que exigem configuração do CORS são incomuns, pois normalmente ocorrem no contexto de [aplicativos de servidor para servidor](../server-to-server.md) e, portanto, não exigem a configuração do CORS. Aplicativos baseados em navegador que exigem configurações do CORS, como [aplicativos de página única](../spa.md) ou [Componentes da Web](../web-component.md), normalmente usam autorização, pois é difícil proteger as credenciais.

Por exemplo, essas duas configurações são definidas da seguinte maneira em uma `CORSPolicyImpl` Configuração de fábrica do OSGi:

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### Exemplo de configuração OSGi

+ [Um exemplo da configuração do OSGi pode ser encontrado no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Configuração do Dispatcher

O Dispatcher do serviço de Publicação (e Pré-visualização) do AEM deve ser configurado para ser compatível com o CORS.

| O cliente se conecta ao | Autor do AEM | AEM Publish | Visualização do AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração do CORS do Dispatcher | ✘ | ✔ | ✔ |

### Permitir cabeçalhos CORS em solicitações HTTP

Atualize o `clientheaders.any` arquivo para permitir cabeçalhos de solicitação HTTP `Origin`,  `Access-Control-Request-Method`, e `Access-Control-Request-Headers` para ser transmitido ao AEM, permitindo que a solicitação HTTP seja processada pelo [Configuração CORS do AEM](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Exemplo de configuração do Dispatcher

+ [Um exemplo do Dispatcher _cabeçalhos do cliente_ A configuração do pode ser encontrada no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Fornecer cabeçalhos de resposta HTTP CORS

Configuração do farm do Dispatcher para armazenamento em cache **Cabeçalhos de resposta HTTP do CORS** para garantir que sejam incluídas quando consultas persistentes do AEM GraphQL forem fornecidas pelo cache do Dispatcher, adicionando o `Access-Control-...` para a lista de cabeçalhos de cache.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

#### Exemplo de configuração do Dispatcher

+ [Um exemplo do Dispatcher _Cabeçalhos de resposta HTTP do CORS_ A configuração do pode ser encontrada no projeto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
