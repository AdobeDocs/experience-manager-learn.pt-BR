---
title: Tratar Envio de Formulário HTML5
description: Criar manipulador de envio de formulário HTML5
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# Tratar Envio de Formulário HTML5

Os formulários HTML5 podem ser enviados ao servlet hospedado no AEM. Os dados enviados podem ser acessados no servlet como um fluxo de entrada. Para enviar o formulário HTML5, é necessário adicionar o &quot;Botão de envio HTTP&quot; ao modelo de formulário usando o AEM Forms Designer

## Criar seu manipulador de envio

Um servlet simples pode ser criado para lidar com o envio do formulário HTML5. Os dados enviados podem ser extraídos usando o código a seguir. Este [servlet](assets/html5-submit-handler.zip) O é disponibilizado para você como parte deste tutorial. Instale o [servlet](assets/html5-submit-handler.zip) usar [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)

O código da linha 9 pode ser usado para chamar o processo J2EE. Verifique se você configurou o [Configuração do SDK do cliente do LiveCycle Adobe](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) se você pretende usar o código para chamar o processo J2EE.

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


## Configure a URL de envio do formulário HTML5

![submit-url](assets/submit-url.PNG)

* Clique no xdp e _Propriedades_->_Avançado_
* copie http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html e cole no campo de texto Enviar URL
* Clique em _SalvarEfechar_ botão.

### Adicionar entrada nos Caminhos de exclusão

* Navegue até [configMgr](http://localhost:4502/system/console/configMgr).
* Pesquisar por _Filtro CSRF do Adobe Granite_
* Adicione a seguinte entrada na seção Caminhos excluídos
* _/content/AemFormsSamples/handlehml5formsubmit_
* Salve as alterações

### Testar o formulário

* Toque no modelo xdp.
* Clique em _Visualizar_->Visualizar como HTML
* Insira alguns dados no formulário e clique em enviar
* Você deve ver os dados enviados gravados no arquivo stdout.log do seu servidor

### Leitura adicional

Este [artigo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) ao gerar PDF a partir do envio do formulário HTML5 também é recomendado.
