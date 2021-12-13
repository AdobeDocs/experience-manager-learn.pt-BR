---
title: Conexões HTTP/HTTPS em portas não padrão para saída de porta flexível
description: Saiba como fazer solicitações HTTP/HTTPS de AEM as a Cloud Service para serviços da Web externos em execução em portas não padrão para saída de porta flexível.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: c53277241e54c757492dbc72e53f89127af389ac
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Conexões HTTP/HTTPS em portas não padrão para saída de porta flexível

As conexões HTTP/HTTPS em portas não padrão (não 80/443) devem ser enviadas por proxy AEM as a Cloud Service, no entanto não precisam de nenhuma `portForwards` e pode usar AEM rede avançada `AEM_PROXY_HOST` e uma porta proxy reservada `AEM_HTTP_PROXY_HOST` ou `AEM_HTTPS_PROXY_HOST` dependendo de é, o destino é HTTP/HTTPS.

## Suporte avançado para rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

| Sem rede avançada | [Saída flexível da porta](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Este exemplo de código é somente para [Aumento Flexível da Porta](../flexible-port-egress.md). Um exemplo de código semelhante, mas diferente, está disponível para [Conexões HTTP/HTTPS em portas não padrão para endereço IP de saída dedicado e VPN](./http-on-non-standard-ports.md).

## Exemplo de código

Este exemplo de código Java™ é de um serviço OSGi que pode ser executado em AEM as a Cloud Service que faz uma conexão HTTP com um servidor da Web externo no 8080. As conexões com servidores da Web HTTPS usam as variáveis de ambiente `AEM_PROXY_HOST` e `AEM_HTTPS_PROXY_PORT` (padrão para `proxy.tunnel:3128` em versões AEM &lt; 6094).

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_HOST") 
        // or System.getenv("AEM_HTTPS_PROXY_HOST"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
            // The explicit fallback of 3128 will be obsoleted in Jan 2022, and only the AEM_HTTP_PROXY_PORT/AEM_HTTPS_PROXY_PORT variable will be required
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", "3128"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
