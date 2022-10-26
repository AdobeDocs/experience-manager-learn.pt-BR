---
title: Gerar FPIDs do Adobe Experience Platform com AEM
description: Saiba como gerar ou atualizar cookies FPID do Adobe Experience Platform usando AEM.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
kt: 11336
thumbnail: kt-11336.jpeg
source-git-commit: aeeed85ec05de9538b78edee67db4d632cffaaab
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---

# Gerar FPIDs do Experience Platform com AEM

Integrar o Adobe Experience Manager (AEM) com o Adobe Experience Platform (AEP) requer AEM para gerar e manter um cookie exclusivo de ID de dispositivo (FPID), para rastrear exclusivamente a atividade do usuário.

Leia a documentação de suporte para [saiba mais sobre os detalhes de como as IDs de dispositivo e as IDs de Experience Cloud de primeira parte trabalham em conjunto](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Abaixo está uma visão geral de como os FPIDs funcionam ao usar o AEM como host da Web.

![FPID e ECIDs com AEM](./assets/aem-platform-fpid-architecture.png)

## Gerar e manter o FPID com AEM

O serviço AEM Publish otimiza o desempenho ao armazenar em cache solicitações o máximo possível, tanto no cache do CDN quanto no AEM Dispatcher.

São solicitações HTTP imperativas que geram o cookie FPID exclusivo por usuário e retornam o valor FPID que nunca são armazenadas em cache e são enviadas diretamente do AEM Publish, que pode implementar a lógica para garantir exclusividade.

Evite gerar o cookie FPID em solicitações de páginas da Web ou outros recursos em cache, pois a combinação do requisito de exclusividade do FPID tornaria esses recursos inacessíveis.

O diagrama a seguir descreve como o serviço de publicação do AEM gerencia os FPIDs.

![FPID e diagrama de fluxo de AEM](./assets/aem-fpid-flow.png)

1. O navegador da Web faz uma solicitação de uma página da Web hospedada pelo AEM. A solicitação pode ser veiculada usando uma cópia em cache da página da Web do cache do CDN ou do Dispatcher AEM.
1. Se a página da Web não puder ser veiculada a partir de caches CDN ou AEM Dispatcher, a solicitação chegará ao serviço de publicação do AEM, que gera a página da Web solicitada.
1. A página da Web é então retornada ao navegador da Web, preenchendo os caches que não puderam servir a solicitação. Com AEM, espere que as taxas de ocorrência do cache do CDN e AEM Dispatcher sejam maiores que 90%.
1. A página da Web contém JavaScript que faz uma solicitação XHR assíncrona (AJAX) inarmazenável em cache para um servlet FPID personalizado no serviço de publicação do AEM. Como essa é uma solicitação que não pode ser armazenada em cache (em virtude do parâmetro de consulta aleatório e cabeçalhos de Controle de Cache), ela nunca é armazenada em cache pelo CDN ou AEM Dispatcher e sempre chega ao serviço de Publicação do AEM para gerar a resposta.
1. O servlet FPID personalizado no serviço de publicação do AEM processa a solicitação, gerando um novo FPID quando nenhum cookie FPID existente é encontrado ou estende a vida de qualquer cookie FPID existente. O servlet também retorna o FPID no corpo da resposta para uso pelo JavaScript do lado do cliente. Felizmente, a lógica personalizada do servlet FPID é leve, impedindo que essa solicitação afete o desempenho do serviço de publicação do AEM.
1. A resposta da solicitação XHR retorna ao navegador com o cookie FPID e FPID como JSON no corpo da resposta para uso pelo SDK da Web da plataforma.

## Amostra de código

O código e a configuração a seguir podem ser implantados no serviço de publicação do AEM para criar um terminal que gera ou estende a vida útil de um cookie FPID existente e retorna o FPID como JSON.

### AEM servlet de cookie FPID

Um endpoint HTTP AEM deve ser criado para gerar ou estender um cookie FPID, usando um [Servlet Sling](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ O servlet está vinculado a `/bin/aem/fpid` como a autenticação não é necessária para acessá-la. Se a autenticação for necessária, vincule-se a um tipo de recurso Sling.
+ O servlet aceita solicitações HTTP GET. A resposta é marcada com `Cache-Control: no-store` para evitar o armazenamento em cache, mas esse endpoint também deve ser solicitado usando parâmetros de consulta exclusivos de armazenamento em cache.

Quando uma solicitação HTTP chega ao servlet, o servlet verifica se um cookie FPID existe na solicitação:

+ Se houver um cookie FPID, estenda a vida do cookie e colete seu valor para gravar na resposta.
+ Se um cookie FPID não existir, gere um novo cookie FPID e salve o valor para gravar na resposta.

O servlet grava o FPID na resposta como um objeto JSON no formulário: `{ fpid: "<FPID VALUE>" }`.

É importante fornecer o FPID ao cliente no corpo, pois o cookie FPID é marcado `HttpOnly`, o que significa que somente o servidor pode ler seu valor, e o JavaScript do lado do cliente não pode.

O valor FPID do corpo da resposta é usado para parametrizar chamadas usando o SDK da Web da plataforma.

Abaixo está o código de exemplo de um ponto de extremidade de servlet AEM (disponível via `HTTP GET /bin/aep/fpid`) que gera ou atualiza um cookie FPID e retorna FPID como JSON.

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

### HTML script

Um JavaScript personalizado do lado do cliente deve ser adicionado à página para chamar de forma assíncrona o servlet, gerar ou atualizar o cookie FPID e retornar o FPID na resposta.

Esse script JavaScript geralmente é adicionado à página usando um dos seguintes métodos:

+ [Tags no Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Biblioteca do cliente AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

A chamada XHR para o servlet FPID de AEM personalizado é rápida, embora assíncrona, por isso é possível que um usuário visite uma página da Web servida por AEM e navegue para fora antes que a solicitação possa ser concluída.
Se isso ocorrer, o mesmo processo tentará novamente o próximo carregamento de página da Web a partir do AEM.

O HTTP GET para o AEM servlet FPID (`/bin/aep/fpid`) é parametrizada com um parâmetro de consulta aleatório para garantir que qualquer infraestrutura entre o navegador e o serviço de publicação do AEM não armazene em cache a resposta da solicitação.
Da mesma forma, a variável `Cache-Control: no-store` o cabeçalho da solicitação é adicionado para oferecer suporte e evitar o armazenamento em cache.

Após uma invocação do servlet FPID AEM, o FPID é recuperado da resposta JSON e usado pelo [SDK da Web da plataforma](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) para enviá-lo para APIs do Experience Platform.

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

### Filtro de permissões do Dispatcher

Por fim, as solicitações HTTP GET para o servlet FPID personalizado devem ser permitidas por meio AEM Dispatcher `filter.any` configuração.

Se essa configuração do Dispatcher não for implementada corretamente, o HTTP GET solicitará para `/bin/aep/fpid` resulta em um 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Recursos do Experience Platform

Consulte a documentação do Experience Platform a seguir para obter IDs de dispositivos primários (FPIDs) e gerenciar dados de identidade com o SDK da Web da plataforma.

+ [Gerar IDs de dispositivo primário](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [IDs de dispositivo próprio no SDK da Web da plataforma](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Dados de identidade no SDK da Web da plataforma](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)


