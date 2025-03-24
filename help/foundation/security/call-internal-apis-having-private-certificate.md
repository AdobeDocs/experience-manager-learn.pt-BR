---
title: Chamar APIs internas com certificados privados
description: Saiba como chamar APIs internas com certificados privados ou autoassinados.
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 332
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
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

Normalmente, esse problema ocorre quando o certificado SSL da **API não é emitido por uma autoridade de certificação reconhecida (CA)** e o aplicativo Java™ não pode validar o certificado SSL/TLS.

Saiba como chamar com êxito APIs com certificados privados ou autoassinados usando o [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) e o **TrustStore global da AEM**.


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

O código usa as classes de biblioteca [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) do [Apache HttpComponent](https://hc.apache.org/) e seus métodos.


## HttpClient e carregar material do AEM TrustStore

Para chamar um ponto de extremidade de API que tenha _certificado privado ou autoassinado_, o `SSLContextBuilder` do [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) deve ser carregado com o TrustStore da AEM e usado para facilitar a conexão.

Siga as etapas abaixo:

1. Faça logon no **AEM Author** como **administrador**.
1. Navegue até **AEM Author > Tools > Security > Trust Store** e abra o **Global Trust Store**. Se estiver acessando pela primeira vez, defina uma senha para o Armazenamento global de confiança.

   ![Repositório Global de Confiança](assets/internal-api-call/global-trust-store.png)

1. Para importar um certificado privado, clique no botão **Selecionar Arquivo de Certificado** e selecione o arquivo de certificado desejado com a extensão `.cer`. Importe clicando no botão **Enviar**.

1. Atualize o código Java™ conforme abaixo. Observe que para usar `@Reference` para obter o `KeyStoreService` do AEM, o código de chamada deve ser um componente/serviço OSGi ou um Modelo Sling (e `@OsgiService` é usado lá).

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

   * Insira o serviço OSGi `com.adobe.granite.keystore.KeyStoreService` do OOTB no componente OSGi.
   * Obtenha o AEM TrustStore global usando `KeyStoreService` e `ResourceResolver`, o método `getAEMTrustStore(...)` faz isso.
   * Crie um objeto de `SSLContextBuilder`, consulte [Detalhes da API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html) do Java™.
   * Carregue o AEM TrustStore global em `SSLContextBuilder` usando o método `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)`.
   * Passe `null` para `TrustStrategy` no método acima. Isso garante que somente os certificados confiáveis do AEM tenham êxito durante a execução da API.


>[!CAUTION]
>
>As chamadas de API com certificados válidos emitidos pela CA falham quando executadas usando a abordagem mencionada. Somente chamadas de API com certificados confiáveis do AEM podem ter êxito ao seguir esse método.
>
>Use a [abordagem padrão](#prototypical-api-invocation-code-using-httpclient) para executar chamadas de API de certificados válidos emitidos por CA, o que significa que somente as APIs associadas a certificados privados devem ser executadas usando o método mencionado anteriormente.

## Evite alterações no JVM Keystore

Uma abordagem convencional para chamar efetivamente APIs internas com certificados privados envolve a modificação da JVM Keystore. Isso é feito importando os certificados privados usando o comando Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549).

No entanto, este método não está alinhado com as práticas recomendadas de segurança e a AEM oferece uma opção superior por meio da utilização do **Armazenamento Global de Confiança** e do [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## Pacote de soluções

O projeto Node.js de amostra rebaixado no vídeo pode ser baixado de [aqui](assets/internal-api-call/REST-APIs.zip).

O código do servlet AEM está disponível na ramificação `tutorial/web-api-invocation` do Projeto do Sites WKND, [consulte](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
