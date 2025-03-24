---
title: Gerar documentos do PDF usando o serviço de saída
description: Mesclar dados com um modelo XDP para gerar PDF não interativos
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 3%

---

# Gerar documentos do PDF usando o serviço de saída

O [Serviço de saída](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) é um serviço OSGi que faz parte do AEM Document Services. Ele é compatível com vários formatos de saída e recursos de design do AEM Forms Designer. O Serviço de saída converte modelos XFA e dados XML para gerar documentos de impressão em diferentes formatos.

O Serviço de saída no AEM Forms as a Cloud Service é muito semelhante ao do AEM Forms 6.5, portanto, se você estiver familiarizado com o uso do Serviço de saída no AEM Forms 6.5, a transição para o AEM Forms as a Cloud Service deve ser simples.

Com o Serviço de saída, é possível criar aplicativos que permitem:

+ Gerar documentos de formulário final preenchendo arquivos de modelo com dados XML.
+ Gerar formulários de saída em vários formatos, incluindo fluxos de impressão PDF, PostScript, PCL e ZPL não interativos.
+ Gerar PDFs de impressão a partir de PDFs de formulário XFA.
+ Gerar documentos PDF, PostScript, PCL e ZPL em massa ao mesclar vários conjuntos de dados com os modelos fornecidos.

Esse serviço foi projetado para ser usado no contexto de uma instância do AEM Forms as a Cloud Service. O trecho de código a seguir gera um documento PDF em um servlet usando o `OutputService`.

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
