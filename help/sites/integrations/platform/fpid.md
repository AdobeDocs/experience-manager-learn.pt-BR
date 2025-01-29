---
title: Gerar FPIDs do Adobe Experience Platform com o AEM Sites
description: Saiba como gerar ou atualizar cookies FPID do Adobe Experience Platform usando o AEM Sites.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2024-10-09T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 266
source-git-commit: 241c56d34c851cf9bac553cb9fc545a835e495d2
workflow-type: tm+mt
source-wordcount: '1054'
ht-degree: 0%

---

# Gerar FPIDs de Experience Platform com o AEM Sites

A integração dos sites do Adobe Experience Manager (AEM) entregues por meio do AEM Publish, com o Adobe Experience Platform AEM (AEP) exige que o gere e mantenha um cookie exclusivo de ID de dispositivo primário (FPID) para rastrear de forma exclusiva a atividade do usuário.

O cookie FPID deve ser definido pelo servidor (AEM Publish) em vez de usar o JavaScript para criar um cookie do lado do cliente. Isso ocorre porque os navegadores modernos, como Safari e Firefox, podem bloquear ou expirar rapidamente os cookies gerados pelo JavaScript.

Leia a documentação de suporte para [saber mais sobre os detalhes de como as IDs de dispositivo de primeira parte e as IDs de Experience Cloud funcionam juntas](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Abaixo está uma visão geral de como os FPIDs funcionam ao usar o AEM como host da Web.

![FPID e ECIDs com AEM](./assets/aem-platform-fpid-architecture.png)

## Gerar e manter o FPID com AEM

O serviço AEM Publish otimiza o desempenho armazenando em cache solicitações o máximo possível, tanto no CDN quanto no AEM Dispatcher caches.

São solicitações HTTP imperativas que geram o cookie FPID único por usuário e retornam o valor FPID que nunca são armazenadas em cache e servidas diretamente do AEM Publish, que pode implementar lógica para garantir exclusividade.

Evite gerar o cookie FPID em solicitações de páginas da Web ou outros recursos armazenáveis em cache, pois a combinação do requisito de exclusividade do FPID tornaria esses recursos não armazenáveis em cache.

O diagrama a seguir descreve como o serviço AEM Publish gerencia FPIDs.

![FPID e diagrama de fluxo do AEM](./assets/aem-fpid-flow.png)

1. O navegador da Web faz uma solicitação para uma página da Web hospedada por AEM. A solicitação pode ser atendida usando uma cópia em cache da página da Web do cache do CDN ou do AEM Dispatcher.
1. Se a página da Web não puder ser disponibilizada a partir dos caches CDN ou AEM Dispatcher, a solicitação chegará ao serviço do AEM Publish, que gera a página da Web solicitada.
1. A página da Web é retornada ao navegador da Web, preenchendo os caches que não puderam atender à solicitação. Com o AEM, espere que as taxas de hit do cache do CDN e AEM Dispatcher sejam maiores que 90%.
1. A página da Web contém JavaScript que faz uma solicitação XHR (AJAX) assíncrona não armazenável em cache para um servlet FPID personalizado no serviço AEM Publish. Como essa é uma solicitação não armazenável em cache (em virtude de seu parâmetro de consulta aleatório e cabeçalhos Cache-Control), ela nunca é armazenada em cache pelo CDN ou pelo AEM Dispatcher e sempre alcança o serviço AEM Publish para gerar a resposta.
1. O servlet FPID personalizado no serviço AEM Publish processa a solicitação, gerando um novo FPID quando nenhum cookie FPID existente é encontrado, ou estende a vida de qualquer cookie FPID existente. O servlet também retorna o FPID no corpo da resposta para uso pelo JavaScript do lado do cliente. Felizmente a lógica de servlet FPID personalizado é leve, evitando que essa solicitação afete o desempenho do serviço AEM Publish.
1. A resposta para a solicitação XHR retorna ao navegador com o cookie FPID e o FPID como JSON no corpo da resposta para uso pelo Platform Web SDK.

## Exemplo de código

O código e a configuração a seguir podem ser implantados no serviço AEM Publish para criar um endpoint que gera ou estende a vida útil de um cookie FPID existente e retorna o FPID como JSON.

### Servlet de cookie AEM Publish FPID

Um ponto de extremidade AEM Publish HTTP deve ser criado para gerar ou estender um cookie FPID, usando um [servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ O servlet está associado a `/bin/aem/fpid`, pois a autenticação não é necessária para acessá-lo. Se a autenticação for necessária, associe a um tipo de recurso do Sling.
+ O servlet aceita solicitações HTTP GET. A resposta está marcada com `Cache-Control: no-store` para impedir o armazenamento em cache, mas esse ponto de extremidade também deve ser solicitado usando parâmetros de consulta exclusivos para substituição de cache.

Quando uma solicitação HTTP atinge o servlet, o servlet verifica se existe um cookie FPID na solicitação:

+ Se existir um cookie FPID, estenda a vida útil do cookie e colete seu valor para gravar na resposta.
+ Se um cookie FPID não existir, gere um novo cookie FPID e salve o valor para gravar na resposta.

O servlet grava o FPID na resposta como um objeto JSON no formato: `{ fpid: "<FPID VALUE>" }`.

É importante fornecer o FPID ao cliente no corpo, pois o cookie FPID está marcado como `HttpOnly`, o que significa que somente o servidor pode ler seu valor, e o JavaScript do lado do cliente não. Para evitar uma nova busca desnecessária do FPID em cada carregamento de página, um cookie `FPID_CLIENT` também é definido, indicando que o FPID foi gerado e expondo o valor ao JavaScript do lado do cliente para uso.

O valor FPID é usado para parametrizar chamadas usando o Platform Web SDK.

Abaixo está um código de exemplo de um endpoint de servlet AEM (disponível via `HTTP GET /bin/aep/fpid`) que gera ou atualiza um cookie FPID e retorna o FPID como JSON.

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
    private static final String CLIENT_COOKIE_NAME = "FPID_CLIENT";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13; // 13 months
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists, get its FPID UUID so its life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the FPID value to the response, either newly generated or the extended one
        // This can be read by the Server (AEM Publish) due to HttpOnly flag.
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");

        // Also set FPID_CLIENT cookie to avoid further server-side FPID generation
        // This can be read by the client-side JavaScript to check if FPID is already generated
        // or if it needs to be requested from server (AEM Publish)
        response.addHeader("Set-Cookie",
                CLIENT_COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "Secure; " + 
                        "SameSite=Lax");

        // Avoid caching the response
        response.addHeader("Cache-Control", "no-store");

        // Return FPID in the response as JSON for client-side access
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);

        response.setContentType("application/json");
        response.getWriter().write(json.toString());
```

### script HTML

Um JavaScript personalizado do lado do cliente deve ser adicionado à página para chamar de forma assíncrona o servlet, gerando ou atualizando o cookie FPID e retornando o FPID na resposta.

Normalmente, esse script do JavaScript é adicionado à página usando um dos seguintes métodos:

+ [Marcas no Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Biblioteca de clientes AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

A chamada XHR para o servlet FPID AEM personalizado é rápida, embora assíncrona, portanto, é possível que um usuário visite uma página da Web fornecida pelo AEM e saia antes que a solicitação seja concluída.
Se isso ocorrer, o mesmo processo tentará novamente no próximo carregamento de página de uma página da Web a partir do AEM.

O HTTP GET para o servlet AEM FPID (`/bin/aep/fpid`) é parametrizado com um parâmetro de consulta aleatória para garantir que qualquer infraestrutura entre o navegador e o serviço AEM Publish não armazene em cache a resposta da solicitação.
Da mesma forma, o cabeçalho de solicitação `Cache-Control: no-store` é adicionado para oferecer suporte à prevenção de armazenamento em cache.

Após a invocação do servlet FPID AEM, o FPID é recuperado da resposta JSON e usado pelo [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) para enviá-lo às APIs Experience Platform.

Consulte a documentação do Experience Platform para obter mais informações sobre [uso de FPIDs no identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Wrap in anonymous function to avoid global scope pollution

    (function() {
        // Utility function to get a cookie value by name
        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }

        // Async function to handle getting the FPID via fetching from AEM, or reading an existing FPID_CLIENT cookie
        async function getFpid() {
            let fpid = getCookie('FPID_CLIENT');
            
            // If FPID can be retrieved from FPID_CLIENT then skip fetching FPID from server
            if (!fpid) {
                // Fetch FPID from the server if no FPID_CLIENT cookie value is present
                try {
                    const response = await fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, {
                        method: 'GET',
                        headers: {
                            'Cache-Control': 'no-store'
                        }
                    });
                    const data = await response.json();
                    fpid = data.fpid;
                } catch (error) {
                    console.error('Error fetching FPID:', error);
                }
            }

            console.log('My FPID is: ', fpid);
            return fpid;
        }

        // Invoke the async function to fetch or skip FPID
        const fpid = await getFpid();

        // Add the fpid to the identityMap in the Platform Web SDK
        // and/or send to AEP via AEP tags or direct AEP Web SDK calls (alloy.js)
    })();
</script>
```

### Filtro de permissão Dispatcher

Por fim, as solicitações HTTP GET para o servlet FPID personalizado devem ser permitidas por meio da configuração `filter.any` do AEM Dispatcher.

Se essa configuração do Dispatcher não for implementada corretamente, as solicitações HTTP GET para `/bin/aep/fpid` resultarão em um erro 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## recursos de Experience Platform

Consulte a documentação do Experience Platform a seguir para IDs de dispositivo primário (FPIDs) e gerenciamento de dados de identidade com o Platform Web SDK.

+ [Gerar IDs de dispositivo primário](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [IDs de dispositivo próprio no Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Dados de identidade no Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
