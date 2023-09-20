---
title: Desenvolver para o Compartilhamento de recursos entre origens (CORS) com AEM
description: Um pequeno exemplo de como aproveitar o CORS para acessar conteúdo AEM de um aplicativo web externo por meio do JavaScript do lado do cliente.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: 1114ec01555baa1c6ffc2ccc5e77165ec9827e4d
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 3%

---

# Desenvolver para o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens)

Um pequeno exemplo de aproveitamento [!DNL CORS] para acessar o conteúdo AEM de um aplicativo web externo por meio do JavaScript do lado do cliente. Este exemplo usa a configuração OSGi do CORS para habilitar o acesso do CORS no AEM. A abordagem de configuração do OSGi é viável quando:

* Uma única origem está acessando o conteúdo de publicação do AEM
* O acesso ao CORS é necessário para o AEM Author

Se o acesso de várias origens ao AEM Publish for necessário, consulte [esta documentação](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

Neste vídeo:

* **www.example.com** mapeia para o host local via `/etc/hosts`
* **aem-publish.local** mapeia para o host local via `/etc/hosts`
* SimpleHTTPServer (um invólucro para [[!DNL Python]SimpleHTTPServer do](https://docs.python.org/2/library/simplehttpserver.html)) está atendendo a página de HTML pela porta 8000.
   * _Não está mais disponível no Mac App Store. Usar semelhante, como [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] está em execução em [!DNL Apache HTTP Web Server] 2.4 e solicitação de proxy reverso para `aem-publish.local` para `localhost:4503`.

Para obter mais detalhes, consulte [Compreensão do CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) no AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JavaScript

Esta página da Web tem lógica de que

1. Ao clicar no botão
1. Cria um [!DNL AJAX GET] solicitação para `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
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

A fábrica de configuração do OSGi para [!DNL Cross-Origin Resource Sharing] está disponível via:

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

Para permitir as [Cabeçalhos de solicitação HTTP para transmitir ao AEM para processamento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), eles devem ter permissão no do Dispatcher `/clientheaders` configuração.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Armazenamento em cache de cabeçalhos de resposta do CORS

Para permitir o armazenamento em cache e a veiculação de cabeçalhos CORS no conteúdo em cache, adicione o seguinte [/cache /configuração de cabeçalhos](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=pt-BR#caching-http-response-headers) ao AEM Publish `dispatcher.any` arquivo.

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

**Reiniciar o aplicativo do servidor Web** depois de fazer alterações no `dispatcher.any` arquivo.

Provavelmente, é necessário limpar totalmente o cache para garantir que os cabeçalhos sejam armazenados em cache corretamente na próxima solicitação após um `/cache /headers` atualização de configuração.

## Materiais de suporte {#supporting-materials}

* [Jeeves para macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.o:qrg/2/library/simplehttpserver.html) (Compatível com Windows/macOS/Linux)

* [Compreensão do CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) no AEM](./understand-cross-origin-resource-sharing.md)
* [Compartilhamento de recursos entre origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
