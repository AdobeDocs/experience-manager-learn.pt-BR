---
title: Gerar FPIDs do Adobe Experience Platform com o AEM Sites
description: Saiba como gerar ou atualizar cookies FPID do Adobe Experience Platform usando o AEM Sites.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 365
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '982'
ht-degree: 0%

---

# Gerar FPIDs de Experience Platform com o AEM Sites

A integração do Adobe Experience Manager (AEM) Sites com o Adobe Experience Platform (AEP) exige que o AEM gere e mantenha um cookie exclusivo de ID de dispositivo primário (FPID) para rastrear exclusivamente a atividade do usuário.

Leia a documentação de suporte para [saiba mais sobre os detalhes de como as IDs de dispositivo de primeira parte e as IDs de Experience Cloud funcionam juntas](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Abaixo está uma visão geral de como os FPIDs funcionam ao usar o AEM como host da Web.

![FPID e ECIDs com AEM](./assets/aem-platform-fpid-architecture.png)

## Gerar e manter o FPID com AEM

O serviço de Publicação do AEM otimiza o desempenho, armazenando solicitações em cache o máximo possível, tanto no CDN quanto no AEM Dispatcher.

As solicitações HTTP imperativas que geram o cookie FPID único por usuário e retornam o valor FPID nunca são armazenadas em cache e fornecidas diretamente do AEM Publish, que pode implementar lógica para garantir a exclusividade.

Evite gerar o cookie FPID em solicitações de páginas da Web ou outros recursos armazenáveis em cache, pois a combinação do requisito de exclusividade do FPID tornaria esses recursos não armazenáveis em cache.

O diagrama a seguir descreve como o serviço de publicação do AEM gerencia os FPIDs.

![Diagrama de fluxo FPID e AEM](./assets/aem-fpid-flow.png)

1. O navegador da Web faz uma solicitação para uma página da Web hospedada por AEM. A solicitação pode ser atendida usando uma cópia em cache da página da Web do cache do CDN ou do AEM Dispatcher.
1. Se a página da Web não puder ser distribuída a partir de caches CDN ou AEM Dispatcher, a solicitação chegará ao serviço de publicação AEM, que gera a página da Web solicitada.
1. A página da Web é retornada ao navegador da Web, preenchendo os caches que não puderam atender à solicitação. Com o AEM, espere que as taxas de acerto de cache do CDN e do AEM Dispatcher sejam maiores que 90%.
1. A página da Web contém JavaScript que faz uma solicitação XHR (AJAX) assíncrona não armazenável em cache para um servlet FPID personalizado no serviço de publicação AEM. Como essa é uma solicitação não armazenável em cache (em virtude de seu parâmetro de consulta aleatório e cabeçalhos de controle de cache), ela nunca é armazenada em cache pelo CDN ou pelo Dispatcher do AEM e sempre alcança o serviço de publicação do AEM para gerar a resposta.
1. O servlet FPID personalizado no serviço de publicação do AEM processa a solicitação, gerando um novo FPID quando nenhum cookie FPID existente é encontrado, ou estende a vida de qualquer cookie FPID existente. O servlet também retorna o FPID no corpo da resposta para uso pelo JavaScript do lado do cliente. Felizmente a lógica de servlet FPID personalizado é leve, evitando que essa solicitação afete o desempenho do serviço de Publicação AEM.
1. A resposta para a solicitação XHR retorna ao navegador com o cookie FPID e o FPID como JSON no corpo da resposta para uso pelo SDK da Web da Platform.

## Exemplo de código

O código e a configuração a seguir podem ser implantados no serviço de publicação do AEM para criar um terminal que gera ou estende a vida útil de um cookie FPID existente e retorna o FPID como JSON.

### Servlet de cookie FPID do AEM

Um endpoint HTTP AEM deve ser criado para gerar ou estender um cookie FPID, usando um [Servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ O servlet está vinculado a `/bin/aem/fpid` como, a autenticação não é necessária para acessá-lo. Se a autenticação for necessária, associe a um tipo de recurso do Sling.
+ O servlet aceita solicitações HTTP GET. A resposta está marcada com `Cache-Control: no-store` para evitar o armazenamento em cache, mas esse endpoint também deve ser solicitado usando parâmetros de consulta exclusivos de eliminação de cache.

Quando uma solicitação HTTP atinge o servlet, o servlet verifica se existe um cookie FPID na solicitação:

+ Se existir um cookie FPID, estenda a vida útil do cookie e colete seu valor para gravar na resposta.
+ Se um cookie FPID não existir, gere um novo cookie FPID e salve o valor para gravar na resposta.

O servlet grava o FPID na resposta como um objeto JSON no formato: `{ fpid: "<FPID VALUE>" }`.

É importante fornecer o FPID ao cliente no corpo, pois o cookie FPID está marcado `HttpOnly`, o que significa que somente o servidor pode ler seu valor, e o JavaScript do lado do cliente não.

O valor FPID do corpo da resposta é usado para parametrizar chamadas usando o SDK da Web da plataforma.

Abaixo está o código de exemplo de um endpoint de servlet AEM (disponível via `HTTP GET /bin/aep/fpid`) que gera ou atualiza um cookie FPID e retorna o FPID como JSON.

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### script HTML

Um JavaScript personalizado do lado do cliente deve ser adicionado à página para chamar de forma assíncrona o servlet, gerando ou atualizando o cookie FPID e retornando o FPID na resposta.

Normalmente, esse script JavaScript é adicionado à página usando um dos seguintes métodos:

+ [Tags no Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Biblioteca cliente AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

A chamada XHR para o servlet FPID AEM personalizado é rápida, embora assíncrona, portanto, é possível que um usuário visite uma página da Web fornecida pelo AEM e saia antes que a solicitação seja concluída.
Se isso ocorrer, o mesmo processo tentará novamente no próximo carregamento de página de uma página da Web a partir do AEM.

O GET HTTP para o servlet AEM FPID (`/bin/aep/fpid`) é parametrizado com um parâmetro de consulta aleatório para garantir que qualquer infraestrutura entre o navegador e o serviço de publicação do AEM não armazene a resposta da solicitação em cache.
Do mesmo modo, a `Cache-Control: no-store` o cabeçalho da solicitação é adicionado para oferecer suporte à prevenção de armazenamento em cache.

Após invocação do servlet FPID AEM, o FPID é recuperado da resposta JSON e usado pelo [SDK da Web da Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) para enviá-lo para APIs Experience Platform.

Consulte a documentação do Experience Platform para obter mais informações sobre [uso de FPIDs no identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Filtro de permissão do Dispatcher

Por fim, as solicitações HTTP GET para o servlet FPID personalizado devem ser permitidas por meio do AEM do Dispatcher `filter.any` configuração.

Se essa configuração do Dispatcher não for implementada corretamente, o HTTP GET solicitará `/bin/aep/fpid` resulta em um 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## recursos de Experience Platform

Consulte a documentação do Experience Platform a seguir para IDs de dispositivo primário (FPIDs) e gerenciamento de dados de identidade com o SDK da Web da plataforma.

+ [Gerar IDs de dispositivo primário](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [IDs de dispositivo próprio no SDK da Web da plataforma](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Dados de identidade no SDK da Web da plataforma](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
