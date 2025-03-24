---
title: Desenvolver para CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) com a AEM
description: Um pequeno exemplo de como aproveitar o CORS para acessar conteúdo do AEM de um aplicativo externo da Web por meio do JavaScript do lado do cliente.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Desenvolver para o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens)

Um pequeno exemplo de como aproveitar o [!DNL CORS] para acessar o conteúdo do AEM a partir de um aplicativo web externo por meio do JavaScript do lado do cliente. Este exemplo usa a configuração OSGi do CORS para habilitar o acesso ao CORS no AEM. A abordagem de configuração do OSGi é viável quando:

* Uma única origem está acessando o conteúdo de publicação do AEM
* O acesso ao CORS é necessário para o AEM Author

Se o acesso de várias origens ao AEM Publish for necessário, consulte [esta documentação](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

Neste vídeo:

* **www.example.com** mapeia para localhost via `/etc/hosts`
* **aem-publish.local** mapeia para localhost via `/etc/hosts`
* SimpleHTTPServer (um wrapper do SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) de [[!DNL Python]) está servindo a página do HTML pela porta 8000.
   * _Não está mais disponível no Mac App Store. Usar similar, como [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] está em execução em [!DNL Apache HTTP Web Server] 2.4 e solicitação de proxy reverso para `aem-publish.local` para `localhost:4503`.

Para obter mais detalhes, consulte [Noções básicas sobre o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) no AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JAVASCRIPT

Esta página da Web tem lógica de que

1. Ao clicar no botão
1. Faz uma solicitação [!DNL AJAX GET] para `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera o `jcr:title` da resposta JSON
1. Injeta o `jcr:title` no DOM

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## Configuração de fábrica do OSGi

A fábrica de Configuração OSGi para [!DNL Cross-Origin Resource Sharing] está disponível via:

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Configuração do Dispatcher {#dispatcher-configuration}

### Permitir cabeçalhos de solicitação CORS

Para permitir que os [cabeçalhos de solicitação HTTP necessários sejam transmitidos para o AEM para processamento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), eles devem ser permitidos na configuração `/clientheaders` do Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Armazenamento em cache de cabeçalhos de resposta do CORS

Para permitir o armazenamento em cache e a veiculação de cabeçalhos CORS no conteúdo em cache, adicione a seguinte configuração [/cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=pt-BR#caching-http-response-headers) ao arquivo `dispatcher.any` de Publicação do AEM.

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

**Reinicie o aplicativo do servidor Web** depois de fazer alterações no arquivo `dispatcher.any`.

Provavelmente, é necessário limpar totalmente o cache para garantir que os cabeçalhos sejam armazenados em cache de maneira adequada na próxima solicitação após uma atualização de configuração `/cache /headers`.

## Materiais de suporte {#supporting-materials}

* [Jeeves para macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (compatível com Windows/macOS/Linux)

* [Noções básicas sobre o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) no AEM](./understand-cross-origin-resource-sharing.md)
* [Compartilhamento de Recursos entre Origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de Acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
