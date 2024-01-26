---
title: Enviar formulário de armazenamento no Armazenamento do Azure
description: Armazenar dados de formulário no Armazenamento do Azure usando a API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
jira: KT-13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
duration: 159
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 0%

---

# Armazenar envios de formulários no Armazenamento do Azure

Este artigo mostra como fazer chamadas REST para armazenar dados AEM Forms enviados no Armazenamento do Azure.
Para poder armazenar dados de formulário enviados no Armazenamento do Azure, as etapas a seguir devem ser seguidas.

## Criar conta de Armazenamento do Azure

[Faça logon na sua conta de portal do Azure e crie uma conta de armazenamento](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Forneça um nome significativo para a conta de armazenamento, clique em Revisar e em Criar. Isso cria sua conta de armazenamento com todos os valores padrão. Para os fins deste artigo, nomeamos nossa conta de armazenamento `aemformstutorial`.


## Criar contêiner

A próxima coisa que precisamos fazer é criar um container para armazenar os dados de envios de formulários.
Na página da conta de armazenamento, clique no item de menu Containers à esquerda e crie um container chamado `formssubmissions`. Verifique se o nível de acesso público está definido como privado
![container](./assets/new-container.png)

## Criar SAS no container

Vamos usar a Assinatura de Acesso Compartilhado ou o Método SAS de autorização para interagir com o contêiner de Armazenamento do Azure.
Navegue até o container na conta de armazenamento, clique nas reticências e selecione a opção Gerar SAS, conforme mostrado na captura de tela
![sas-on-container](./assets/sas-on-container.png)
Certifique-se de especificar as permissões apropriadas e a data final apropriada, como mostrado na captura de tela abaixo, e clique em Gerar token SAS e URL. Copie o token SAS do Blob e o url SAS do Blob. Usaremos esses dois valores para fazer nossas chamadas HTTP
![chaves de acesso compartilhado](./assets/shared-access-signature.png)


## Fornecer o token SAS de blob e o URI de armazenamento

Para tornar o código mais genérico, as duas propriedades podem ser configuradas usando a configuração OSGi, como mostrado abaixo. A variável _**aemformstutorial**_ é o nome da conta de armazenamento, _**envios de formulários**_ é o container no qual os dados serão armazenados.
![osgi-configuration](./assets/azure-portal-osgi-configuration.png)


## Criar solicitação PUT

A próxima etapa é criar uma solicitação PUT para armazenar os dados de formulário enviados no Armazenamento do Azure. Todo envio de formulário precisa ser identificado por uma ID de BLOB exclusiva. O ID de BLOB exclusivo geralmente é criado em seu código e inserido no url da solicitação PUT.
Veja a seguir o URL parcial da solicitação PUT. A variável `aemformstutorial` é o nome da conta de armazenamento, formsubmissions é o contêiner no qual os dados serão armazenados com uma ID de BLOB exclusiva. O restante do URL permanecerá o mesmo.
https://aemformstutorial.blob.core.windows.net/formsubmissions/blobid/sastoken A função a seguir é gravada para armazenar os dados do formulário enviado no Armazenamento do Azure usando uma solicitação PUT. Observe o uso do nome do container e da uuid no url. Você pode criar um serviço OSGi ou um servlet sling usando o código de amostra listado abaixo e armazenar os envios de formulários no Armazenamento do Azure.

```java
 public String saveFormDatainAzure(String formData) {
    log.debug("in SaveFormData!!!!!" + formData);
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");
    httpPut.addHeader("Content-Type", "text/plain");

    try {
        httpPut.setEntity(new StringEntity(formData));

        CloseableHttpResponse response = httpClient.execute(httpPut);
        log.debug("Response code " + response.getStatusLine().getStatusCode());
        if (response.getStatusLine().getStatusCode() == 201) {
            return uuid.toString();
        }
    } catch (IOException e) {
        log.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;

}
```

## Verificar dados armazenados no container

![formulário-dados-em-contêiner](./assets/form-data-in-container.png)

## Testar a solução

* [Implantar o pacote OSGi personalizado](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importe o modelo de formulário adaptável personalizado e o componente de página associado ao modelo](./assets/store-and-fetch-from-azure.zip)

* [Importar a amostra de formulário adaptável](./assets/bank-account-sample-form.zip)

* Especifique os valores apropriados na Configuração do Portal do Azure usando o console de configuração OSGi
* [Pré-visualizar e enviar o formulário Conta Bancária](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Verifique se os dados estão armazenados no contêiner de armazenamento do Azure de sua escolha. Copie a ID do blob.
* [Visualizar o formulário Conta Bancária](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) e especifique a ID de Blob como um parâmetro de guid na URL para que o formulário seja pré-preenchido com os dados do armazenamento do Azure

