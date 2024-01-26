---
title: Acionar o fluxo de trabalho do AEM no envio do formulário HTM5 - Criar perfil personalizado
description: Continue preenchendo o formulário para publicação de conteúdo para dispositivos móveis no modo offline e envie o formulário para publicação de conteúdo para dispositivos móveis para acionar o fluxo de trabalho do AEM
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 101
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Criar perfil personalizado

Nesta parte, criaremos um [perfil personalizado.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Um perfil é responsável por renderizar o XDP como HTML. Um perfil padrão é fornecido imediatamente para renderizar XDPs como HTML. Ele representa uma versão personalizada do serviço de representação do Mobile Forms. Você pode usar o serviço de Representação de formulários para dispositivos móveis para personalizar a aparência, o comportamento e as interações do Forms para dispositivos móveis. Em nosso perfil personalizado, capturaremos os dados preenchidos no formulário móvel usando a API do guidebridge. Esses dados são enviados para o servlet personalizado que gerará um PDF interativo e o transmitirá ao aplicativo de chamada.

Obtenha os dados do formulário usando o `formBridge` API do JavaScript. Utilizamos o `getDataXML()` método:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

No método do manipulador de sucesso, fazemos uma chamada para o servlet personalizado em execução no AEM. Este servlet renderizará e retornará o pdf interativo com os dados do formulário para dispositivos móveis

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Gerar PDF interativo

Veja a seguir o código do servlet responsável por renderizar o pdf interativo e devolvê-lo ao aplicativo de chamada. O servlet chama `mobileFormToInteractivePdf` método do serviço OSGi personalizado do DocumentServices.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
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
        // TODO Add proper error logging
      }
    }
}
```

### Renderizar PDF interativo

O código a seguir usa o [API de serviço do Forms](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) para renderizar o PDF interativo com os dados do formulário para dispositivos móveis.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

Para visualizar a capacidade de baixar o PDF interativo do formulário para dispositivos móveis parcialmente preenchido, [clique aqui](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Depois que o PDF é baixado, a próxima etapa é enviar o PDF para acionar um workflow de AEM. Esse fluxo de trabalho mesclará os dados do PDF enviado e gerará um PDF não interativo para revisão.

O perfil personalizado criado para este caso de uso está disponível como parte deste tutorial de ativos.
