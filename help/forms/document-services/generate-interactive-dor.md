---
title: Gerar DoR interativa com dados de Formulário adaptável
description: Mesclar dados de formulário adaptáveis com o modelo XDP para gerar PDF baixável
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
duration: 258
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# Baixar DoR interativa

Um caso de uso comum é o de ser capaz de baixar uma DoR interativa com os dados do Formulário adaptável. A DoR baixada será concluída usando o Adobe Acrobat ou o Adobe Reader.

## O formulário adaptável não se baseia no esquema XSD

Se o XDP e o formulário adaptável não estiverem baseados em nenhum esquema, siga as etapas a seguir para gerar um Documento de registro interativo.

### Criar formulário adaptável

Crie um formulário adaptável e verifique se os nomes dos campos do formulário adaptável são idênticos aos nomes dos campos no modelo xdp.
Anote o nome do elemento raiz do seu modelo xdp.
![root- element](assets/xfa-root-element.png)

### Biblioteca do cliente

O código a seguir é acionado quando o botão Baixar PDF é acionado

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

## Formulário adaptável com base no esquema XSD

Se o xdp não for baseado em XSD, siga as etapas a seguir para criar XSD(schema) no qual você baseará seu formulário adaptável

### Gerar dados de amostra para o XDP

* Abra o XDP no AEM Forms Designer.
* Clique em Arquivo | Propriedades do formulário | Visualização
* Clique em Gerar dados de visualização
* Clique em Gerar
* Forneça um nome de arquivo significativo, como &quot;form-data.xml&quot;

### Gerar XSD a partir dos dados xml

Você pode usar qualquer uma das ferramentas online gratuitas para [gerar XSD](https://www.freeformatter.com/xsd-generator.html) dos dados xml gerados na etapa anterior.

### Criar formulário adaptável

Crie um formulário adaptável com base no XSD da etapa anterior. Associe o formulário para usar a biblioteca do cliente &quot;irs&quot;. Essa biblioteca cliente tem o código para fazer uma chamada de POST para o servlet que retorna o PDF para o aplicativo de chamada. O código a seguir é acionado quando o _Baixar PDF_ foi clicado

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
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
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
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

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

No código de amostra, extraímos o Nome xdp e outros parâmetros do objeto de solicitação. Se o formulário não for baseado em XSD, o documento xml a ser mesclado com o xdp será criado. Se o formulário for baseado em XSD, simplesmente extrairmos o nó apropriado do formulário adaptável enviado para gerar o documento xml a ser mesclado com o modelo xdp.

## Implantar a amostra no servidor

Para testar isso no servidor local, siga as seguintes etapas:

1. [Baixe e instale o pacote DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Adicione a seguinte entrada no serviço de mapeador de usuário do Apache Sling Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Baixe e instale o pacote DocumentServices personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Ele tem o servlet para mesclar os dados com o modelo XDP e transmitir o pdf de volta
1. [Importar a biblioteca do cliente](assets/generate-interactive-dor-client-lib.zip)
1. [Importar os ativos do artigo (formulário adaptável, modelos XDP e XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Preencha alguns dos campos de formulário.
1. Clique em Baixar PDF para obter o PDF. Talvez seja necessário aguardar alguns segundos para baixar o PDF.

>[!NOTE]
>
>Você pode tentar o mesmo caso de uso com [formulário adaptável não baseado em xsd](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Transmita os parâmetros apropriados para o endpoint de publicação no streampdf.js localizado na biblioteca de clientes irs.
