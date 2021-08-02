---
title: Acionar fluxo de trabalho AEM no envio de formulário HTML5
seo-title: Acionar fluxo de trabalho AEM no envio do formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
feature: Formulários para publicação de conteúdo para dispositivos móveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---


# Criar perfil personalizado

Nesta parte, criaremos um perfil personalizado [.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Um perfil é responsável pela renderização do XDP como HTML. Um perfil padrão é fornecido imediatamente para renderização de XDP como HTML. Ele representa uma versão personalizada do Mobile Forms Rendition Service. Você pode usar o serviço Representação de formulário móvel para personalizar a aparência, o comportamento e as interações do Forms móvel. No nosso perfil personalizado, capturaremos os dados preenchidos no formulário móvel usando a API do guidebridge. Esses dados são enviados para o servlet personalizado que gerará um PDF interativo e o redirecionará para o aplicativo que faz a chamada.

Obtenha os dados do formulário usando a `formBridge` API do JavaScript. Usamos o método `getDataXML()`:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

No método do manipulador de sucesso, fazemos uma chamada para o servlet personalizado em execução no AEM. Este servlet renderizará e retornará o pdf interativo com os dados do formulário móvel

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

A seguir, o código do servlet que é responsável pela renderização do pdf interativo e pelo retorno do pdf para o aplicativo que faz a chamada. O servlet chama o método `mobileFormToInteractivePdf` do serviço OSGi DocumentServices personalizado.

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

O código a seguir usa a [API do serviço do Forms](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) para renderizar o PDF interativo com os dados do formulário móvel.

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

Para visualizar a capacidade de baixar o PDF interativo a partir do formulário móvel parcialmente preenchido, [clique aqui](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Depois que o PDF for baixado, a próxima etapa é enviar o PDF para acionar um fluxo de trabalho de AEM. Esse workflow unirá os dados do PDF enviado e gerará um PDF não interativo para revisão.

O perfil personalizado criado para este caso de uso está disponível como parte desses ativos tutoriais.
