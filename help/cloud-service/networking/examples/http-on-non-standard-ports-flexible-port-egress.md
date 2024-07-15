---
title: Conexões HTTP/HTTPS em portas fora do padrão para saída de porta flexível
description: Saiba como fazer solicitações HTTP/HTTPS do AEM as a Cloud Service para serviços Web externos em execução em portas não padrão para Saída flexível de porta.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
duration: 86
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Conexões HTTP/HTTPS em portas fora do padrão para saída de porta flexível

As conexões HTTP/HTTPS em portas fora do padrão (não 80/443) devem ser enviadas por proxy do AEM as a Cloud Service, no entanto, elas não precisam de regras `portForwards` especiais e podem usar o `AEM_PROXY_HOST` da rede avançada AEM e uma porta de proxy reservada `AEM_HTTP_PROXY_PORT` ou `AEM_HTTPS_PROXY_PORT`, dependendo se o destino é HTTP/HTTPS.

## Suporte avançado a rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

Verifique se a configuração avançada de rede [apropriada](../advanced-networking.md#advanced-networking) foi definida antes de seguir este tutorial.

| Sem rede avançada | [Saída de porta flexível](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede Virtual Privada](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Este exemplo de código é somente para [Saída de porta flexível](../flexible-port-egress.md). Um exemplo de código semelhante, mas diferente, está disponível para [conexões HTTP/HTTPS em portas não padrão para VPN e endereço IP de saída dedicado](./http-dedicated-egress-ip-vpn.md).

## Exemplo de código

Este exemplo de código Java™ é de um serviço OSGi que pode ser executado no AEM as a Cloud Service que faz uma conexão HTTP com um servidor Web externo no 8080. As conexões com servidores Web HTTPS usam as variáveis de ambiente `AEM_PROXY_HOST` e `AEM_HTTPS_PROXY_PORT` (padrão para `proxy.tunnel:3128` em versões AEM &lt; 6094).

>[!NOTE]
> Recomenda-se que as [APIs HTTP do Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) sejam usadas para fazer chamadas HTTP/HTTPS do AEM.

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

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
