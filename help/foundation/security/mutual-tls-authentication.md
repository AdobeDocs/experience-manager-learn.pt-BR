---
title: Autenticação mTLS (Mutual Transport Layer Security) da AEM
description: Saiba como fazer chamadas HTTPS do AEM para APIs da Web que exigem autenticação mTLS (Mutual Transport Layer Security).
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 495
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Autenticação mTLS (Mutual Transport Layer Security) da AEM

Saiba como fazer chamadas HTTPS do AEM para APIs da Web que exigem autenticação mTLS (Mutual Transport Layer Security).

>[!VIDEO](https://video.tv.adobe.com/v/3447864?quality=12&learn=on&captions=por_br)

A autenticação mTLS ou TLS bidirecional melhora a segurança do protocolo TLS, exigindo que **o cliente e o servidor se autentiquem**. Essa autenticação é feita usando certificados digitais. Normalmente, é usada em cenários nos quais a segurança e a verificação de identidade fortes são críticas.

Por padrão, ao tentar fazer uma conexão HTTPS com uma API da Web que requer autenticação mTLS, a conexão falha com o erro:

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

Esse problema ocorre quando o cliente não apresenta um certificado para autenticação.

Saiba como chamar com êxito APIs que exigem autenticação mTLS usando o [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) e o **KeyStore e TrustStore da AEM**.


## HttpClient e carregar material do AEM KeyStore

Em um nível superior, as seguintes etapas são necessárias para chamar uma API protegida por mTLS do AEM.

### Geração de certificado do AEM

Solicite o certificado do AEM fazendo parceria com a equipe de segurança de sua organização. A equipe de segurança fornece ou solicita detalhes relacionados ao certificado, como chave, solicitação de assinatura do certificado (CSR) e, usando a CSR, o certificado é emitido.

Para fins de demonstração, gere os detalhes relacionados ao certificado, como chave, solicitação de assinatura do certificado (CSR). No exemplo abaixo, uma CA autoassinada é usada para emitir o certificado.

- Primeiro, gere o certificado interno da autoridade de certificação (CA).

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- Gere o certificado do AEM.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- Converta a chave privada do AEM para o formato DER. O KeyStore da AEM requer a chave privada no formato DER.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>Os certificados CA autoassinados são usados apenas para fins de desenvolvimento. Para produção, use uma Autoridade de Certificação (CA) confiável para emitir o certificado.


### Intercâmbio de certificados

Se estiver usando uma CA autoassinada para o certificado do AEM, como acima, envie o certificado ou o certificado interno da CA (Autoridade de Certificação) para o provedor de API.

Além disso, se o provedor de API estiver usando um certificado de CA autoassinado, receba o certificado ou o certificado de CA (Autoridade de Certificação) interno do provedor de API.

### Importação de certificado

Para importar o certificado do AEM, siga as etapas abaixo:

1. Faça logon no **AEM Author** como **administrador**.

1. Navegue até **AEM Author > Ferramentas > Segurança > Usuários > Criar ou Selecionar um usuário existente**.

   ![Criar ou selecionar um usuário existente](assets/mutual-tls-authentication/create-or-select-user.png)

   Para fins de demonstração, um novo usuário chamado `mtl-demo-user` é criado.

1. Para abrir as **Propriedades do Usuário**, clique no nome do usuário.

1. Clique na guia **Armazenamento de chaves** e no botão **Criar armazenamento de chaves**. Em seguida, na caixa de diálogo **Definir senha de acesso do KeyStore**, defina uma senha para o keystore desse usuário e clique em Salvar.

   ![Criar Armazenamento De Chaves](assets/mutual-tls-authentication/create-keystore.png)

1. Na nova tela, na seção **ADICIONAR CHAVE PRIVADA DO ARQUIVO DER**, siga as etapas abaixo:

   1. Inserir alias

   1. Importe a chave privada do AEM no formato DER, gerado acima.

   1. Importe os arquivos da Cadeia de certificados, gerados acima.

   1. Clique em Enviar

      ![Importar chave privada do AEM](assets/mutual-tls-authentication/import-aem-private-key.png)

1. Verifique se o certificado foi importado com êxito.

   ![Chave privada e certificado do AEM importados](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

Se o provedor de API estiver usando um certificado CA autoassinado, importe o certificado recebido para o TrustStore da AEM, siga as etapas de [aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html?lang=pt-BR#httpclient-and-load-aem-truststore-material).

Da mesma forma, se o AEM estiver usando um certificado CA autoassinado, solicite ao provedor da API que o importe.

### Protótipo de código de invocação da API mTLS usando HttpClient

Atualize o código Java™ conforme abaixo. Para usar a anotação `@Reference` para obter o serviço `KeyStoreService` do AEM, o código de chamada deve ser um componente/serviço OSGi ou um Modelo Sling (e `@OsgiService` é usado lá).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- Insira o serviço OSGi `com.adobe.granite.keystore.KeyStoreService` do OOTB no componente OSGi.
- Obtenha o AEM KeyStore do usuário usando `KeyStoreService` e `ResourceResolver`, o método `getAEMKeyStore(...)` faz isso.
- Se o provedor de API estiver usando um certificado CA autoassinado, obtenha o AEM TrustStore global, o método `getAEMTrustStore(...)` fará isso.
- Crie um objeto de `SSLContextBuilder`, consulte [Detalhes da API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html) do Java™.
- Carregue o AEM KeyStore do usuário em `SSLContextBuilder` usando o método `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)`.
- A senha do keystore é a senha que foi definida ao criar o keystore, ela deve ser armazenada na configuração OSGi, consulte [Valores de Configuração Secreta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=pt-BR#secret-configuration-values).

## Evite alterações no JVM Keystore

Uma abordagem convencional para invocar efetivamente APIs mTLS com certificados privados envolve a modificação da JVM Keystore. Isso é feito importando os certificados privados usando o comando Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549).

No entanto, este método não está alinhado com as práticas recomendadas de segurança e a AEM oferece uma opção superior por meio da utilização do **KeyStores específicos do usuário e do [KeyStoreStore** e do KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).

## Pacote de soluções

O projeto Node.js de amostra rebaixado no vídeo pode ser baixado de [aqui](assets/internal-api-call/REST-APIs.zip).

O código do servlet AEM está disponível na ramificação `tutorial/web-api-invocation` do Projeto do Sites WKND, [consulte](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
