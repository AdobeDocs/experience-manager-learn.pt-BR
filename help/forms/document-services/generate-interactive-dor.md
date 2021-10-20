---
title: Gerar DoR interativo com dados do formulário adaptável
description: Mesclar dados de formulário adaptáveis com modelo XDP para gerar pdf baixável
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---


# Download Interativo DoR

Um caso de uso comum é ser capaz de baixar um DoR interativo com os dados do Formulário adaptável. O DoR baixado será concluído usando o Adobe Acrobat ou Adobe Reader.

Para realizar esse caso de uso, precisamos fazer o seguinte

## Gerar dados de amostra para o XDP

* Abra o XDP no designer do AEM Forms.
* Clique em Arquivo | Propriedades do formulário | Pré-visualização
* Clique em Gerar dados de visualização
* Clique em Gerar
* Forneça um nome de arquivo significativo, como &quot;form-data.xml&quot;

## Gerar XSD a partir dos dados xml

Você pode usar qualquer uma das ferramentas online gratuitas para [gerar XSD](https://www.freeformatter.com/xsd-generator.html) dos dados xml gerados na etapa anterior.

## Criar formulário adaptável

Crie um formulário adaptável com base no XSD da etapa anterior. Associe o formulário para usar a biblioteca cliente &quot;irs&quot;. Essa biblioteca do cliente tem o código para fazer uma chamada de POST para o servlet que retorna o PDF para o aplicativo chamador O código a seguir é acionado quando o _Baixar o PDF_ é clicado

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## Criar servlet personalizado

Crie um servlet personalizado que mesclará os dados com o modelo XDP e retornará o pdf. O código para fazer isso está listado abaixo. O servlet personalizado faz parte do [Pacote AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

No código de amostra, o nome do modelo (f8918-r14e_redo-barcode_3 2.xdp) é codificado permanentemente. Você pode facilmente passar o nome do modelo para o servlet para tornar esse código genérico para funcionar em todos os modelos.


## Implante a amostra no servidor

Para testar isso em seu servidor local, siga as seguintes etapas:

1. [Baixe e instale o pacote DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Adicione a seguinte entrada no Serviço Mapeador de Usuário do Apache Sling Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Baixe e instale o pacote personalizado DocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Isso tem o servlet para unir os dados ao modelo XDP e fazer o stream do pdf de volta
1. [Importe a biblioteca do cliente](assets/irs.zip)
1. [Importar o formulário adaptável](assets/f8918complete.zip)
1. [Importar o modelo e o esquema XDP](assets/xdp-template-and-xsd.zip)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Preencha alguns campos de formulário
1. Clique em Baixar PDF para obter o PDF. Talvez seja necessário aguardar alguns segundos para que o PDF baixe


