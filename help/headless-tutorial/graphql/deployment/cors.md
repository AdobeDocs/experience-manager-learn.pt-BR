---
title: Configuração do CORS para AEM GraphQL
description: Saiba como configurar o CORS (Cross-origin resource sharing) para uso com AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 2%

---


# Compartilhamento de recursos entre origens (CORS)

O CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos de várias origens) da Adobe Experience Manager as a Cloud Service facilita propriedades da Web que não são AEM para fazer chamadas do lado do cliente baseadas em navegador para AEM APIs GraphQL.

>[!TIP]
>
> As configurações a seguir são exemplos. Ajuste-os para alinhar-se às necessidades do seu projeto.



## Requisito de CORS

O CORS é necessário para conexões baseadas em navegador com AEM APIs GraphQL, quando o cliente que se conecta a AEM NÃO é atendido da mesma origem (também conhecida como host ou domínio) AEM.

| Tipo de cliente | Aplicativo de página única (SPA) | Componente da Web/JS | Móvel | Servidor para servidor |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requer configuração de CORS | ✔ | ✔ | ✘ | ✘ |

## Configuração do OSGi

A fábrica de configuração AEM do CORS OSGi define os critérios de permissão para aceitar solicitações HTTP do CORS.

| O cliente se conecta a | Autor do AEM | AEM Publish | Visualização de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Exige configuração OSGi do CORS | ✔ | ✔ | ✔ |


O exemplo abaixo define uma configuração OSGi para publicação no AEM (`../config.publish/..`), mas pode ser adicionado a [qualquer pasta runmode suportada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

As principais propriedades de configuração são:

+ `alloworigin` e/ou `alloworiginregexp` especifica a(s) origem(ões) em que o cliente que se conecta a AEM web é executado.
+ `allowedpaths` especifica os padrões de caminho de URL permitidos nas origens especificadas. Para oferecer suporte AEM consultas persistentes de GraphQL, use o seguinte padrão: `"/graphql/execute.json.*"`
+ `supportedmethods` especifica os métodos HTTP permitidos para as solicitações do CORS. Adicionar `GET`, para suportar consultas persistentes de GraphQL AEM.

[Saiba mais sobre a configuração OSGi do CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)


Essa configuração de exemplo suporta o uso de consultas persistentes de GraphQL AEM. Para usar consultas GraphQL definidas pelo cliente, adicione um URL de ponto de extremidade GraphQL em `allowedpaths` e `POST` para `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
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
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```


## Configuração do Dispatcher

O Dispatcher do serviço de publicação (e visualização) do AEM deve ser configurado para oferecer suporte ao CORS.

| O cliente se conecta a | Autor do AEM | Publicação do AEM | Visualização de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requer a configuração do CORS do Dispatcher | ✘ | ✔ | ✔ |

### Permitir cabeçalhos CORS em solicitações HTTP

Atualize o `clientheaders.any` para permitir cabeçalhos de solicitação HTTP `Origin`,  `Access-Control-Request-Method` e `Access-Control-Request-Headers` para ser transmitido ao AEM, permitindo que a solicitação HTTP seja processada pelo [Configuração AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### Entregar cabeçalhos de resposta HTTP CORS

Configurar o farm do dispatcher para armazenar em cache **Cabeçalhos de resposta HTTP CORS** para garantir que sejam incluídos quando AEM consultas persistentes de GraphQL forem veiculadas a partir do cache do Dispatcher, adicionando a variável `Access-Control-...` cabeçalhos para a lista de cabeçalhos de cache.

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
