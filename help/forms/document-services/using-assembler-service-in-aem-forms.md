---
title: Uso do serviço Assembler no AEM Forms
seo-title: Uso do serviço Assembler no AEM Forms
description: Uso do Assembler Service no AEM Forms para montar vários arquivos pdf
seo-description: Uso do Assembler Service no AEM Forms para montar vários arquivos pdf
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---


# Uso do serviço Assembler no AEM Forms{#using-assembler-service-in-aem-forms}

Este artigo fornece os ativos para demonstrar a capacidade de arrastar e soltar vários arquivos PDF no navegador e salvar o arquivo pdf montado em seu sistema de arquivos. A seguir está o código do servlet que monta os arquivos pdf carregados usando o navegador.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

Para obter esse recurso funcionando no servidor AEM

* Baixe o [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) no sistema local.
* Carregar e instalar o pacote usando o gerenciador de [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Pacote[DownloadCustom Documento Services](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Baixar o [desenvolvimento com o pacote de usuários de serviço](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Implantar e start os pacotes usando o console da Web [felix](http://localhost:4502/system/console/bundles)
* Aponte seu navegador para [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Arraste e solte alguns arquivos de arquivos PDF

>[!NOTE]
>
>Verifique se a instalação do AEM Forms foi concluída. Todos os seus pacotes precisam estar no estado ativo.
>
>Verifique se você adicionou - as bibliotecas delegadas de inicialização RSA e BouncyCastle, conforme mencionado nesta [Instalação do AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Avisos para esta demonstração**
>
> * O código não lida com documentos PDF baseados em XFA
   >
   > 
* Certifique-se de arrastar e soltar somente arquivos PDF
>
>







