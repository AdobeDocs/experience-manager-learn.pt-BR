---
title: Exibir documento de registro em linha
description: Mescle dados de formulário adaptáveis com o modelo XDP e exiba o PDF em linha usando a API de pdf incorporada da Document Cloud.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 165
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# Exibir DoR em linha

Um caso de uso comum é exibir um documento pdf com os dados inseridos pelo preenchimento de formulário.

Para concluir este caso de uso, utilizamos a [API de Incorporação do Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

As etapas a seguir foram executadas para concluir a integração

## Criar componente personalizado para exibir o PDF em linha

Um componente personalizado (pdf incorporado) foi criado para incorporar o pdf retornado pela chamada POST.

## Biblioteca do cliente

O código a seguir é executado quando o botão de caixa de seleção `viewPDF` é clicado. Passamos os dados do formulário adaptável, o nome do modelo, para o endpoint para gerar o pdf. O pdf gerado é exibido no preenchimento de formulário usando a biblioteca JavaScript de pdf incorporada.

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## Gerar dados de amostra para o XDP

* Abra o XDP no AEM Forms Designer.
* Clique em Arquivo | Propriedades do formulário | Visualizar
* Clique em Gerar dados de visualização
* Clique em Gerar
* Forneça um nome de arquivo significativo, como &quot;form-data.xml&quot;

## Gerar XSD a partir dos dados xml

Você pode usar qualquer uma das ferramentas online gratuitas para [gerar XSD](https://www.freeformatter.com/xsd-generator.html) a partir dos dados xml gerados na etapa anterior.

## Fazer upload do modelo

Carregue o modelo xdp no [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) usando o botão criar


## Criar formulário adaptável

Crie um formulário adaptável com base no XSD da etapa anterior.
Adicione uma nova guia ao adaptável. Adicionar um componente caixa de seleção e componente pdf incorporado a esta guia
Nomeie a caixa de seleção viewPDF.
Configure o componente de pdf incorporado como mostrado na captura de tela abaixo
![pdf incorporado](assets/embed-pdf-configuration.png)

**Incorporar Chave de API do PDF** - Esta é a chave que você pode usar para incorporar o pdf. Essa chave só funcionará com localhost. Você pode criar [sua própria chave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) e associá-la a outro domínio.

**Ponto de extremidade retornando o pdf** - Este é o servlet personalizado que mesclará os dados com o modelo xdp e retornará o pdf.

**Nome do modelo** - Este é o caminho para o xdp. Normalmente, ele é armazenado na pasta formulários e documentos.

**Nome do Arquivo PDF** - Esta é a cadeia de caracteres que aparecerá no componente pdf incorporado.

## Criar servlet personalizado

Um servlet personalizado foi criado para mesclar os dados com o modelo XDP e retornar o pdf. O código para fazer isso está listado abaixo. O servlet personalizado faz parte do [pacote embutido](assets/embedpdf.core-1.0-SNAPSHOT.jar)

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
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

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## Implantar a amostra no servidor

Para testar isso no servidor local, siga as seguintes etapas:

1. [Baixe e instale o pacote de pdf incorporado](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Ele tem o servlet para unir os dados ao modelo XDP e transmitir o pdf de volta.
1. Adicione o caminho /bin/getPDFToEmbed na seção de caminhos excluídos do Filtro CSRF do Adobe Granite usando o [AEM ConfigMgr](http://localhost:4502/system/console/configMgr). É recomendável usar a [estrutura de proteção do CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=pt-BR) em seu ambiente de produção
1. [Importar a biblioteca do cliente e o componente personalizado](assets/embed-pdf.zip)
1. [Importar o formulário e o modelo adaptáveis](assets/embed-pdf-form-and-xdp.zip)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Preencha alguns dos campos de formulário
1. Vá até a guia Exibir PDF. Marque a caixa de seleção exibir pdf. Você deve ver um pdf exibido no formulário preenchido com os dados do formulário adaptável
