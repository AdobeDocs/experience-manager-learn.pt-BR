---
title: Conexões HTTP/HTTPS em portas não padrão para endereço IP de saída dedicado e VPN
description: Saiba como fazer solicitações HTTP/HTTPS de AEM as a Cloud Service para serviços da Web externos em execução em portas não padrão para endereço IP de saída dedicado e VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# Conexões HTTP/HTTPS em portas não padrão para endereço IP de saída dedicado e VPN

As conexões HTTP/HTTPS em portas não padrão (não 80/443) devem ser enviadas por proxy AEM as a Cloud Service, no entanto não precisam de nenhuma `portForwards` e pode usar AEM rede avançada `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`e `AEM_HTTPS_PROXY_PORT`.

## Suporte avançado para rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

Verifique se a variável [adequada](../advanced-networking.md#advanced-networking) a configuração avançada de rede foi configurada antes de seguir este tutorial.

| Sem rede avançada | [Saída flexível da porta](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Este exemplo de código é somente para [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) e [VPN](../vpn.md). Um exemplo de código semelhante, mas diferente, está disponível para [Conexões HTTP/HTTPS em portas não padrão para saída de porta flexível](./http-on-non-standard-ports-flexible-port-egress.md).

## Exemplo de código

Este exemplo de código Java™ é de um serviço OSGi que pode ser executado em AEM as a Cloud Service que faz uma conexão HTTP com um servidor da Web externo no 8080. As conexões com servidores da Web HTTPS usam o `AEM_HTTPS_PROXY_HOST` e `AEM_HTTPS_PROXY_PORT` em vez de  `AEM_HTTP_PROXY_HOST` e `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> Recomenda-se que [APIs HTTP do Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) são usadas para fazer chamadas HTTP/HTTPS do AEM.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // If the URL is http, use System.getenv("AEM_HTTP_PROXY_HOST") and System.getenv("AEM_HTTP_PROXY_PORT")
        // Else if the URL is https, us System.getenv("AEM_HTTPS_PROXY_HOST") and System.getenv("AEM_HTTPS_PROXY_PORT")

        if (System.getenv("AEM_HTTP_PROXY_HOST") != null) {
            // Create a ProxySelector that maps to AEM's provided AEM_HTTP_PROXY_HOST and AEM_HTTP_PROXY_PORT
            ProxySelector proxySelector = ProxySelector.of(
                    new InetSocketAddress(System.getenv("AEM_HTTP_PROXY_HOST"),
                            Integer.parseInt(System.getenv("AEM_HTTP_PROXY_PORT"))));
            // Create an HttpClient and provide the proxy selector that will use AEM's native HTTP proxy configuration
            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_HTTP_PROXY");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_HTTP_PROXY");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
