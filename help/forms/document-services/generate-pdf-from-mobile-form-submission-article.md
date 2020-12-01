---
title: Gerar PDF a partir do envio de formulário HTM5
seo-title: Gerar PDF a partir do envio de formulário HTML5
description: Gerar PDF a partir do envio de formulário móvel
seo-description: Gerar PDF a partir do envio de formulário móvel
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Gerar PDF a partir do envio de formulário HTM5 {#generate-pdf-from-htm-form-submission}

Este artigo o guiará pelas etapas envolvidas na geração de pdf a partir de uma submissão de formulário HTML5(aka Mobile Forms). Essa demonstração também explicará as etapas necessárias para adicionar uma imagem ao formulário HTML5 e mesclar a imagem no pdf final.

Para ver uma demonstração ao vivo desse recurso, visite o [servidor de amostra](https://forms.enablementadobe.com/content/samples/samples.html?query=0) e procure &quot;Formulário móvel para PDF&quot;.

Para unir os dados enviados ao modelo xdp, fazemos o seguinte

Escreva um servlet para lidar com o envio de formulário HTML5

* Dentro deste servlet obtenha os dados enviados
* Mesclar esses dados com o modelo xdp para gerar o pdf
* Transmitir o pdf de volta ao aplicativo de chamada

A seguir está o código do servlet que extrai os dados enviados da solicitação. Em seguida, ele chama o método documentServices .mobileFormToPDF personalizado para obter o pdf.

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

Para adicionar imagem ao formulário móvel e exibi-la no pdf, usamos o seguinte

Modelo XDP - No modelo xdp, adicionamos um campo de imagem e um botão chamado btnAddImage. O código a seguir manipula o evento click de btnAddImage em nosso perfil personalizado. Como você pode ver, acionamos o evento click file1. Nenhum código é necessário no xdp para realizar esse caso de uso

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Perfil](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles) personalizado. O uso de perfis personalizados facilita a manipulação de objetos HTML DOM do formulário móvel. Um elemento de arquivo oculto é adicionado ao arquivo HTML.jsp. Quando o usuário clica em &quot;Adicionar sua foto&quot;, acionamos o evento click do elemento file. Isso permite que o usuário navegue e selecione a fotografia a ser anexada. Em seguida, usamos o objeto FileReader javascript para obter a string codificada base64 da imagem. A string de imagem base64 é armazenada no campo de texto no formulário. Quando o formulário é enviado, extraímos esse valor e o inserimos no elemento img do XML. Esse XML é usado para unir ao xdp para gerar o pdf final.

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

O código acima é executado quando acionamos o evento click do elemento file. Na linha 5, extraímos o conteúdo do arquivo carregado como string base64 e armazenamos no campo de texto. Esse valor é então extraído quando o formulário é submetido ao nosso servlet.

Em seguida, configuramos as seguintes propriedades (avançadas) do formulário móvel no AEM

* Enviar URL - http://localhost:4502/bin/handlemobileformsubmission. Este é nosso servlet que unirá os dados enviados ao modelo xdp
* PERFIL de renderização HTML - certifique-se de selecionar &quot;AddImageToMobileForm&quot;. Isso acionará o código para adicionar uma imagem ao formulário.

Para testar esse recurso em seu próprio servidor, siga as seguintes etapas:

* [Implantar o pacote AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implantar o pacote Desenvolvimento com Usuário do Serviço](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Baixe e instale o pacote associado a este artigo.](assets/pdf-from-mobile-form-submission.zip)

* Certifique-se de que o URL de envio e o perfil de renderização HTML estejam definidos corretamente ao exibir a página de propriedades de [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Pré-visualização do XDP como html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Adicione uma imagem ao formulário e envie-a. Você deve recuperar o PDF com a imagem nele.

