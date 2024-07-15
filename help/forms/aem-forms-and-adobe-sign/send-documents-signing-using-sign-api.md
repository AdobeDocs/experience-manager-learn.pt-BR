---
title: Utilização da API do Adobe Sign no AEM Forms
description: Enviar documentos para assinatura usando métodos auxiliares do Adobe Sign
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Uso de métodos auxiliares do Adobe Sign

Em certos casos de uso, talvez seja necessário enviar um documento para assinaturas sem usar um fluxo de trabalho do AEM. Nesses casos, será muito conveniente usar os métodos de invólucro expostos pelo pacote de amostra fornecido neste artigo.

## Implante o pacote OSGi de amostra

[Implante o pacote OSGi](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) por meio do Console da Web AEM OSGi. Especifique a chave de integração de API e o usuário de API usando a configuração OSGi, como mostrado abaixo, por meio do Gerenciador de configurações do console da Web OSGi do AEM.

 Observe que o pacote OSGi `AdobeSignHelperMethods` não é reconhecido como um código de produto Adobe Experience Manager (AEM) e, portanto, não é suportado pelo Suporte Adobe.
![configuração de assinatura](assets/sign-configuration.png)


## Documentação da API

Os itens a seguir estão disponíveis por meio do serviço OSGi `AcrobatSignHelperMethods` fornecido no pacote OSGi.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


O documento usado para criar um contrato ou um formulário web. O documento é carregado primeiro no Acrobat Sign pelo remetente. Isso é mencionado como _transitório_, pois está disponível para uso somente por 7 dias após o carregamento. Este método aceita `com.adobe.aemfd.docmanager.Document` e retorna a ID de documento transitória.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

Envie o documento para assinatura usando a ID do documento transitório para assinatura para o usuário identificado pelo parâmetro de email.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Um widget é como um modelo reutilizável que pode ser apresentado aos usuários várias vezes e assinado várias vezes. Use este método para obter a ID do widget usando a ID do documento transitório.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

Obter o URL de um widget para um ID de widget específico. Esse URL do widget pode ser apresentado aos usuários para assinar o documento.

## Usar a API

O `AcrobatSignHelperMethods` é um serviço OSGi, portanto, deve ser anotado usando a anotação @Reference no seu código java.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
