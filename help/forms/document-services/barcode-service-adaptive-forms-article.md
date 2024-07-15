---
title: Serviço De Código De Barras Com O Adaptive Forms
description: Usar o serviço de código de barras para decodificar código de barras e preencher campos de formulário a partir dos dados extraídos.
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# Serviço De Código De Barras Com O Adaptive Forms{#barcode-service-with-adaptive-forms}

Este artigo demonstrará o uso do Serviço de código de barras para preencher o Formulário adaptável. O caso de uso é o seguinte:

1. O usuário adiciona PDF com código de barras como anexo do Formulário adaptável
1. O caminho do anexo é enviado para o servlet
1. O servlet decodificou o código de barras e retorna os dados no formato JSON
1. O Formulário adaptável é preenchido usando os dados decodificados

O código a seguir decodifica o código de barras e preenche um objeto JSON com os valores decodificados. O servlet retorna o objeto JSON em sua resposta ao aplicativo de chamada.



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

Veja a seguir o código do servlet. Esse servlet é chamado quando o usuário adiciona um anexo ao Formulário adaptável. O servlet retorna o objeto JSON de volta para o aplicativo de chamada. O aplicativo de chamada preenche o formulário adaptável com os valores extraídos do objeto JSON.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

O código a seguir faz parte da biblioteca do cliente que é referenciada pelo Formulário adaptável. Quando um usuário adiciona o anexo ao formulário adaptável, esse código é acionado. O código faz uma chamada GET para o servlet com o caminho do anexo passado no parâmetro de solicitação. Os dados recebidos da chamada de servlet são usados para preencher o formulário adaptável.

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>O formulário adaptável incluído neste pacote foi criado usando o AEM Forms 6.4. Se você pretende usar esse pacote no ambiente AEM Forms 6.3, crie o Formulário adaptável no AEM Form 6.3

Linha 12 - Código personalizado para obter resolvedor de serviço. Este pacote está incluído como parte dos ativos deste artigo.

Linha 23 - Chamar o método extractBarCode do DocumentServices para obter o objeto JSON preenchido com dados decodificados

Para executar isso em seu sistema, siga as seguintes etapas:

1. [Baixe BarcodeService.zip](assets/barcodeservice.zip) e importe para AEM usando o gerenciador de pacotes
1. [Baixe e instale o pacote de serviços de documento personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Baixe e instale o pacote DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Baixe o formulário de PDF de amostra](assets/barcode.pdf)
1. Aponte seu navegador para o [formulário adaptável de amostra](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Carregar o PDF de amostra fornecido
1. Você deve ver os formulários preenchidos com os dados
