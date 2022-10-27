---
title: Manipular envio de formulário do HTML5
description: Criar manipulador de envio de formulário HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 3%

---

# Manipular envio de formulário do HTML5

Formulários HTML5 podem ser enviados para servlet hospedado em AEM. Os dados enviados podem ser acessados no servlet como um fluxo de entrada. Para enviar o formulário HTML5, é necessário adicionar &quot;Botão Enviar por HTTP&quot; no modelo de formulário usando o AEM Forms Designer

## Criar seu manipulador de envio

Um servlet simples pode ser criado para lidar com o envio do formulário HTML5. Os dados enviados podem então ser extraídos usando o seguinte código. Essa [servlet](assets/html5-submit-handler.zip) O é disponibilizado para você como parte deste tutorial. Instale o [servlet](assets/html5-submit-handler.zip) usar [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)

O código da linha 9 pode ser usado para invocar o processo J2EE. Certifique-se de que você configurou [Configuração do SDK do cliente do Adobe LiveCycle](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) se você pretende usar o código para invocar o processo J2EE.

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
* Clique em _SaveAndClose_ botão.

### Adicionar entrada em Excluir caminhos

* Navegar para [configMgr](http://localhost:4502/system/console/configMgr).
* Procurar por _Filtro CSRF do Adobe Granite_
* Adicione a seguinte entrada na seção Caminhos excluídos
* _/content/AemFormsSamples/handlehml5formsubmit_
* Salve as alterações

### Testar o formulário

* Toque no modelo xdp.
* Clique em _Visualizar_->Visualizar como HTML
* Insira alguns dados no formulário e clique em enviar
* Você deve ver os dados enviados gravados no arquivo stdout.log do seu servidor

### Leitura adicional

Essa [artigo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) também é recomendável gerar PDF a partir do envio do formulário HTML5.
