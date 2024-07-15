---
title: Enviar formulário de armazenamento no Armazenamento do Azure
description: Armazenar dados de formulário no Armazenamento do Azure usando a API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 78
exl-id: 77f93aad-0cab-4e52-b0fd-ae5af23a13d0
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Buscar dados do armazenamento do Azure

Este artigo mostra como preencher um formulário adaptável com os dados armazenados no armazenamento do Azure.
Pressupõe-se que você tenha armazenado os dados do formulário adaptável no armazenamento do Azure e agora queira preencher previamente seu formulário adaptável com esses dados.
>[!NOTE]
>O código deste artigo não funciona com os componentes principais baseados em formulário adaptável.[O artigo equivalente para o formulário adaptável baseado em componente principal está disponível aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/prefill-form-with-data-attachments/introduction.html?lang=en)


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

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Quando um formulário adaptável é renderizado com um parâmetro guid no url, o componente de página personalizado associado ao modelo busca e preenche o formulário adaptável com os dados do armazenamento do Azure.
Este é o código na jsp do componente Página associado ao modelo

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

* [Especifique os valores apropriados na Configuração do Portal do Azure usando o console de configuração OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/some-useful-integrations/store-form-data-in-azure-storage.html?lang=en#provide-the-blob-sas-token-and-storage-uri)

* [Visualizar e enviar o formulário BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Verifique se os dados estão armazenados no contêiner de armazenamento do Azure de sua escolha. Copie a ID do blob.

* [Visualize o formulário BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) e especifique a ID Blob como um parâmetro guid na URL para o formulário a ser preenchido com os dados do armazenamento do Azure
