---
title: Desenvolver para o CORS (Cross-Origin Resource Sharing, compartilhamento de recursos entre origens) com AEM
description: Um exemplo curto de como aproveitar o CORS para acessar AEM conteúdo de um aplicativo Web externo por meio do JavaScript do lado do cliente.
version: 6.3, 6,4, 6.5
topic: Segurança, desenvolvimento
role: Developer
level: Beginner
feature: Segurança
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---


# Desenvolver para o CORS (Cross-Origin Resource Sharing, compartilhamento de recursos de várias origens)

Um exemplo curto de como aproveitar [!DNL CORS] para acessar AEM conteúdo de um aplicativo Web externo por meio do JavaScript do lado do cliente.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

Neste vídeo:

* **www.example.** commaps to localhost via  `/etc/hosts`
* **aem-publish.** localmaps to localhost via  `/etc/hosts`
* [O SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)  (um wrapper para o  [[!DNL Python]SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) do) está disponibilizando a página HTML pela porta 8000.
* [!DNL AEM Dispatcher] O está em execução na  [!DNL Apache HTTP Web Server] 2.4 e a solicitação de proxy reverso  `aem-publish.local` para  `localhost:4503`.

Para obter mais detalhes, consulte [Compreender o CORS (Cross-Origin Resource Sharing) em AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML e JavaScript

Essa página da Web tem lógica de que

1. Ao clicar no botão
1. Faz uma solicitação [!DNL AJAX GET] para `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera o `jcr:title` da resposta JSON
1. Injeta `jcr:title` no DOM

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

## Configuração de fábrica OSGi

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

Para permitir o armazenamento em cache e a veiculação de cabeçalhos CORS em conteúdo em cache, adicione a seguir [/clientheaders configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) a todos os arquivos `dispatcher.any` de publicação do AEM de suporte.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Reinicie o** aplicativo do servidor da Web depois de fazer alterações no  `dispatcher.any` arquivo.

Provavelmente, a limpeza total do cache é necessária para garantir que os cabeçalhos sejam armazenados em cache adequadamente na próxima solicitação após uma atualização de configuração `/clientheaders`.

## Materiais de apoio {#supporting-materials}

* [AEM Fábrica de configuração OSGi para Políticas de Compartilhamento de Recursos entre Origens](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer para macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)  (compatível com Windows/macOS/Linux)

* [Como entender o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) no AEM](./understand-cross-origin-resource-sharing.md)
* [Compartilhamento de recursos entre origens (W3C)](https://www.w3.org/TR/cors/)
* [Controle de acesso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

