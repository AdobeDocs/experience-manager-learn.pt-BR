---
title: Processar envio de formulário HTML5
description: Criar manipulador de envio de formulário HTML5
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---


# Processar envio de formulário HTML5

Formulários HTML5 podem ser enviados para o servlet hospedado em AEM. Os dados enviados podem ser acessados no servlet como um fluxo de entrada. Para enviar o formulário HTML5, é necessário adicionar &quot;Botão Enviar por HTTP&quot; no modelo de formulário usando o AEM Forms Designer

## Criar seu manipulador de envio

Um servlet simples pode ser criado para lidar com o envio de formulário HTML5. Os dados enviados podem ser extraídos usando o seguinte código. Este [servlet](assets/html5-submit-handler.zip) é disponibilizado para você como parte deste tutorial. Instale o [servlet](assets/html5-submit-handler.zip) usando [gerenciador de pacote](http://localhost:4502/crx/packmgr/index.jsp)

O código da linha 9 pode ser usado para chamar o processo J2EE. Certifique-se de ter configurado [Configuração do SDK do Cliente do LiveCycle do Adobe](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) se pretende utilizar o código para invocar o processo J2EE.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## Configurar o URL de envio do formulário HTML5

![submit-url](assets/submit-url.PNG)

* Toque em xdp e clique em _Propriedades_->_Avançado_
* copie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html e cole isso no campo de texto Enviar URL
* Clique no botão _SaveAndClose_.

### Adicionar entrada nos Caminhos de exclusão

* Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
* Procure _Filtro CSRF de Adobe Granite_
* Adicione a seguinte entrada na seção Caminhos excluídos
* _/content/AemFormsSamples/handlehml5formsubmit_
* Salvar suas alterações

### Testar o formulário

* Toque no modelo xdp.
* Clique em _Pré-visualização_->Pré-visualização como HTML
* Insira alguns dados no formulário e clique em enviar
* Você deve ver os dados enviados gravados no arquivo stdout.log do seu servidor

### Leitura adicional

Este [artigo](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) sobre como gerar PDF a partir do envio de formulário HTML5 também é recomendado.




