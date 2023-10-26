---
title: Enviar formulário de armazenamento no Armazenamento do Azure
description: Armazenar dados de formulário no Armazenamento do Azure usando a API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Buscar dados do armazenamento do Azure

Este artigo mostra como preencher um formulário adaptável com os dados armazenados no armazenamento do Azure.
Pressupõe-se que você tenha armazenado os dados do formulário adaptável no armazenamento do Azure e agora queira preencher previamente seu formulário adaptável com esses dados.

## Criar solicitação GET

A próxima etapa é gravar o código para buscar os dados do Armazenamento do Azure usando a blobID. O código a seguir foi gravado para buscar os dados. O URL foi construído usando os valores sasToken e storageURI da configuração OSGi e o blobID passado para a função getBlobData

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.error("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.error("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Quando um formulário adaptável é renderizado com um `guid` parâmetro na URL, o componente de página personalizado associado ao modelo busca e preenche o formulário adaptável com os dados do armazenamento do Azure.
O componente de página associado ao modelo tem o seguinte código JSP.

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## Testar a solução

* [Implantar o pacote OSGi personalizado](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importe o modelo de formulário adaptável personalizado e o componente de página associado ao modelo](./assets/store-and-fetch-from-azure.zip)

* [Importar a amostra de formulário adaptável](./assets/bank-account-sample-form.zip)

* Especifique os valores apropriados na Configuração do Portal do Azure usando o console de configuração OSGi
* [Pré-visualizar e enviar o formulário Conta Bancária](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Verifique se os dados estão armazenados no contêiner de armazenamento do Azure de sua escolha. Copie a ID do blob.
* [Visualizar o formulário Conta Bancária](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) e especifique a ID de Blob como um parâmetro de guid na URL para que o formulário seja pré-preenchido com os dados do armazenamento do Azure

