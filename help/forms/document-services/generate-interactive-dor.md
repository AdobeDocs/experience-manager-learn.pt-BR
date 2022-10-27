---
title: Gerar DoR interativo com dados do formulário adaptável
description: Mesclar dados de formulário adaptáveis com modelo XDP para gerar pdf baixável
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 1%

---

# Download Interativo DoR

Um caso de uso comum é ser capaz de baixar um DoR interativo com os dados do Formulário adaptável. O DoR baixado será concluído usando o Adobe Acrobat ou Adobe Reader.

## O formulário adaptável não se baseia no esquema XSD

Se o XDP e o Formulário adaptável não forem baseados em nenhum esquema, siga as etapas a seguir para gerar um Documento de registro interativo.

### Criar formulário adaptável

Crie um formulário adaptável e verifique se os nomes de campo do formulário adaptável são idênticos aos nomes de campo no modelo xdp.
Anote o nome do elemento raiz do seu modelo xdp.
![elemento raiz](assets/xfa-root-element.png)

### Client Lib

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

## Formulário adaptável baseado no esquema XSD

Se o xdp não for baseado no XSD, siga as etapas a seguir para criar o XSD(schema) no qual você baseará seu formulário adaptável

### Gerar dados de amostra para o XDP

* Abra o XDP no designer do AEM Forms.
* Clique em Arquivo | Propriedades do formulário | Pré-visualização
* Clique em Gerar dados de visualização
* Clique em Gerar
* Forneça um nome de arquivo significativo, como &quot;form-data.xml&quot;

### Gerar XSD a partir dos dados xml

Você pode usar qualquer uma das ferramentas online gratuitas para [gerar XSD](https://www.freeformatter.com/xsd-generator.html) dos dados xml gerados na etapa anterior.

### Criar formulário adaptável

Crie um formulário adaptável com base no XSD da etapa anterior. Associe o formulário para usar a biblioteca cliente &quot;irs&quot;. Essa biblioteca do cliente tem o código para fazer uma chamada de POST para o servlet que retorna o PDF para o aplicativo chamador O código a seguir é acionado quando o _Baixar o PDF_ é clicado

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

No código de amostra, extraímos o Nome xdp e outros parâmetros do objeto de solicitação. Se o formulário não for baseado em XSD, o documento xml a ser mesclado com o xdp será criado. Se o formulário for baseado em XSD, simplesmente extraímos o nó apropriado dos dados enviados do formulário adaptável para gerar o documento xml para mesclar com o modelo xdp.

## Implante a amostra no servidor

Para testar isso em seu servidor local, siga as seguintes etapas:

1. [Baixe e instale o pacote DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Adicione a seguinte entrada no Serviço Mapeador de Usuário do Apache Sling Service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Baixe e instale o pacote personalizado DocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Isso tem o servlet para unir os dados ao modelo XDP e fazer o stream do pdf de volta
1. [Importe a biblioteca do cliente](assets/generate-interactive-dor-client-lib.zip)
1. [Importe os ativos do artigo (Formulário adaptativo, modelos XDP e XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [Visualizar formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Preencha alguns dos campos do formulário.
1. Clique em Baixar PDF para obter o PDF. Talvez seja necessário aguardar alguns segundos para que o PDF baixe.

>[!NOTE]
>
>Você pode experimentar o mesmo caso de uso com [formulário adaptável não baseado em xsd](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Passe os parâmetros apropriados para o terminal de publicação em streampdf.js localizado na clientlib deles.
