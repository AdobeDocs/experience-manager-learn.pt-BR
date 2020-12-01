---
title: Acionar fluxo de trabalho AEM no envio de formulário HTM5
seo-title: Acionar fluxo de trabalho AEM no envio de formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---


# Criar Perfil personalizado

Nesta parte, criaremos um perfil personalizado [.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Um perfil é responsável por renderizar o XDP como HTML. Um perfil padrão é fornecido fora da caixa para renderizar XDP como HTML. Representa uma versão personalizada do serviço Mobile Forms Rendition. Você pode usar o serviço de representação de formulário móvel para personalizar a aparência, o comportamento e as interações do Mobile Forms. Em nosso perfil personalizado, capturaremos os dados preenchidos no formulário móvel usando a API guidebridge. Esses dados são então enviados para o servlet personalizado que gerará um PDF interativo e os enviará de volta para o aplicativo chamador.

Obtenha os dados do formulário usando a API JavaScript `formBridge`. Usamos o método `getDataXML()`:

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

A seguir está o código do servlet responsável pela renderização do pdf interativo e pelo retorno do pdf para o aplicativo chamador. O servlet chama o método `mobileFormToInteractivePdf` do serviço OSGi personalizado DocumentServices.

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

O código a seguir usa a [Forms Service API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) para renderizar o PDF interativo com os dados do formulário móvel.

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

Para visualização da capacidade de fazer download de PDF interativo a partir de um formulário móvel parcialmente preenchido, [clique aqui](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content).
Após o download do PDF, a próxima etapa é enviar o PDF para acionar um fluxo de trabalho AEM. Esse fluxo de trabalho mesclará os dados do PDF enviado e gerará um PDF não interativo para revisão.

O perfil personalizado criado para esse caso de uso está disponível como parte dos ativos do tutorial.
