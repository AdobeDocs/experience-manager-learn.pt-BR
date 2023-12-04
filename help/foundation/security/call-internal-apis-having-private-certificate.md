---
title: Chamar APIs internas com certificados privados
description: Saiba como chamar APIs internas com certificados privados ou autoassinados.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 391
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

# Chamar APIs internas com certificados privados

Saiba como fazer chamadas HTTPS do AEM para APIs da Web usando certificados privados ou autoassinados.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

Por padrão, ao tentar fazer uma conexão HTTPS com uma API da Web que usa um certificado autoassinado, a conexão falha com o erro:

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

Normalmente, esse problema ocorre quando a variável **O certificado SSL da API não é emitido por uma autoridade de certificação reconhecida** O e o aplicativo Java™ não podem validar o certificado SSL/TLS.

Saiba como chamar com êxito APIs com certificados privados ou autoassinados usando [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) e **AEM global TrustStore**.


## Código de invocação de API de protótipo usando HttpClient

O código a seguir faz uma conexão HTTPS com uma API da Web:

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

O código usa o [Apache HttpComponent](https://hc.apache.org/)do [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) classes de biblioteca e seus métodos.


## HttpClient e carregar material AEM TrustStore

Para chamar um endpoint de API que tenha _certificado privado ou autoassinado_, o [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)do `SSLContextBuilder` deve ser carregado com AEM TrustStore e usado para facilitar a conexão.

Siga as etapas abaixo:

1. Fazer logon em **Autor do AEM** como um **administrador**.
1. Navegue até **Autor do AEM > Ferramentas > Segurança > Armazenamento de confiança** e abra a variável **Armazenamento global de confiança**. Se estiver acessando pela primeira vez, defina uma senha para o Armazenamento global de confiança.

   ![Armazenamento global de confiança](assets/internal-api-call/global-trust-store.png)

1. Para importar um certificado privado, clique em **Selecionar arquivo de certificado** e selecione o arquivo de certificado desejado com `.cer` extensão. Importe clicando em **Enviar** botão.

1. Atualize o código Java™ conforme abaixo. Observe que para usar `@Reference` para obter AEM `KeyStoreService` o código de chamada deve ser um componente/serviço OSGi ou um Modelo Sling (e `@OsgiService` é usado lá).

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * Injetar o OOTB `com.adobe.granite.keystore.KeyStoreService` Serviço OSGi no componente OSGi.
   * Obtenha o AEM TrustStore global usando o `KeyStoreService` e `ResourceResolver`, o `getAEMTrustStore(...)` faz isso.
   * Criar um objeto de `SSLContextBuilder`, consulte Java™ [Detalhes da API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * Carregue o AEM TrustStore global em `SSLContextBuilder` usar `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` método.
   * Aprovado `null` para `TrustStrategy` no método acima, ele garante que somente certificados confiáveis de AEM sejam bem-sucedidos durante a execução da API.


>[!CAUTION]
>
>As chamadas de API com certificados válidos emitidos pela CA falham quando executadas usando a abordagem mencionada. Somente chamadas de API com certificados confiáveis AEM podem ter êxito ao seguir esse método.
>
>Use o [método padrão](#prototypical-api-invocation-code-using-httpclient) para executar chamadas de API de certificados válidos emitidos por CA, o que significa que somente APIs associadas a certificados privados devem ser executadas usando o método mencionado anteriormente.

## Evite alterações no JVM Keystore

Uma abordagem convencional para chamar efetivamente APIs internas com certificados privados envolve a modificação da JVM Keystore. Isso é feito importando os certificados privados usando o Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) comando.

No entanto, esse método não está alinhado às práticas recomendadas de segurança e o AEM oferece uma opção superior por meio da utilização do **Armazenamento global de confiança** e [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## Pacote de soluções

O projeto Node.js de amostra demoed no vídeo pode ser baixado em [aqui](assets/internal-api-call/REST-APIs.zip).

O código do servlet AEM está disponível no site do projeto WKND `tutorial/web-api-invocation` filial, [consulte](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
