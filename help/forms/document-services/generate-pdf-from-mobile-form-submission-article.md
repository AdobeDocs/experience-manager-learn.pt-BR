---
title: Gerar PDF a partir do envio de formulário HTM5
description: Gerar PDF a partir do envio de formulário para dispositivos móveis
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# Gerar PDF a partir do envio de formulário HTM5 {#generate-pdf-from-htm-form-submission}

Este artigo o guiará pelas etapas relativas à geração de pdf a partir do envio de um formulário HTML5(também conhecido como Forms móvel). Esta demonstração também explicará as etapas necessárias para adicionar uma imagem ao formulário HTML5 e mesclar a imagem no pdf final.


Para unir os dados enviados ao modelo xdp, fazemos o seguinte

Escreva um servlet para lidar com o envio do formulário HTML5

* Dentro deste servlet, obtenha os dados enviados
* Mesclar estes dados com o modelo xdp para gerar o pdf
* Transmita o pdf de volta para o aplicativo chamador

Veja a seguir o código do servlet que extrai os dados enviados da solicitação. Em seguida, ele chama o método documentServices .mobileFormToPDF personalizado para obter o PDF.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

Para adicionar imagem ao formulário móvel e exibir essa imagem no pdf, usamos o seguinte

Modelo XDP - no modelo xdp, adicionamos um campo de imagem e um botão chamado btnAddImage. O código a seguir lida com o evento de clique da imagem btnAddImage em nosso perfil personalizado. Como você pode ver, acionamos o evento de clique file1. Nenhuma codificação é necessária no xdp para realizar esse caso de uso

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Perfil personalizado](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). Usar o perfil personalizado facilita a manipulação de objetos DOM HTML do formulário móvel. Um elemento de arquivo oculto é adicionado ao HTML.jsp. Quando o usuário clica em &quot;Adicionar sua foto&quot;, acionamos o evento de clique do elemento do arquivo. Isso permite que o usuário navegue e selecione a fotografia a ser anexada. Em seguida, usamos o objeto FileReader do javascript para obter a string codificada em base64 da imagem. A cadeia de caracteres da imagem base64 é armazenada no campo de texto do formulário. Quando o formulário é enviado, extraímos esse valor e o inserimos no elemento img do XML. Esse XML é usado para mesclar com o xdp para gerar o pdf final.

O perfil personalizado usado para este artigo foi disponibilizado para você como parte dos ativos deste artigo.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

O código acima é executado quando acionamos o evento de clique do elemento do arquivo. Linha 5 extraímos o conteúdo do arquivo carregado como base64 string e o armazenamos no campo de texto. Esse valor é então extraído quando o formulário é enviado ao nosso servlet.

Em seguida, configuramos as seguintes propriedades (avançadas) do nosso formulário móvel no AEM

* Enviar URL - http://localhost:4502/bin/handlemobileformsubmission. Este é nosso servlet que mesclará os dados enviados com o modelo xdp
* Perfil de renderização do HTML - Selecione &quot;AddImageToMobileForm&quot;. Isso acionará o código para adicionar imagem ao formulário.

Para testar esse recurso em seu próprio servidor, siga as seguintes etapas:

* [Implantar o pacote AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implantar o pacote Desenvolvimento com usuário de serviço](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e instale o pacote associado a este artigo.](assets/pdf-from-mobile-form-submission.zip)

* Verifique se o URL de envio e o perfil de Renderização do HTML estão definidos corretamente, visualizando a página de propriedades do  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Visualizar o XDP como html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Adicione uma imagem ao formulário e envie. Você deve ter PDF de volta com a imagem nele.
