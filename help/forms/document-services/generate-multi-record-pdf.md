---
title: Geração de vários pdfs de um arquivo de dados
description: O OutputService fornece vários métodos para criar documentos usando um design de formulário e dados para mesclar com o design de formulário. Saiba como gerar vários pdfS de um xml grande que contém vários registros individuais.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 0%

---

# Gerar um conjunto de Documentos PDF a partir de um arquivo de dados xml

O OutputService fornece vários métodos para criar documentos usando um design de formulário e dados para mesclar com o design de formulário. O artigo a seguir explica o caso de uso para gerar vários pdfS a partir de um xml grande que contém vários registros individuais.
Esta é a captura de tela do arquivo xml que contém vários registros.

![xml de vários registros](assets/multi-record-xml.PNG)

O xml de dados tem 2 registros. Cada registro é representado pelo elemento form1. Este xml é passado para o OutputService [método generatePDFOutputBatch](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) obtemos a lista de documentos pdf (um por registro) A assinatura do método generatePDFOutputBatch utiliza os seguintes parâmetros

* templates - Mapa contendo o template, identificado por uma chave
* dados - Mapa contendo documentos de dados xml, identificados por chave
* pdfOutputOptions - opções para configurar a geração de pdf
* batchOptions - opções para configurar lote



## Detalhes do caso de uso{#use-case-details}

Nesse caso de uso, vamos fornecer uma interface da Web simples para fazer upload do modelo e do arquivo de dados (xml). Quando o upload dos arquivos estiver concluído e a solicitação POST for enviada para o servlet AEM. Este servlet extrai os documentos e chama o método generatePDFOutputBatch do OutputService. Os pdf gerados são compactados em um arquivo zip e disponibilizados ao usuário final para download no navegador da Web.

## Código de servlet{#servlet-code}

Veja a seguir o trecho de código do servlet. O código extrai o modelo (xdp) e o arquivo de dados (xml) da solicitação. O arquivo de modelo é salvo no sistema de arquivos. Dois mapas são criados - templateMap e dataFileMap, que contêm o modelo e os arquivos xml(data), respectivamente. Em seguida, é feita uma chamada para o método generateMultipleRecords do serviço DocumentServices.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### Código de implementação da interface{#Interface-Implementation-Code}

O código a seguir gera vários pdf usando generatePDFOutputBatch do OutputService e retorna um arquivo zip contendo os arquivos pdf para o servlet de chamada

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### Implantar no servidor{#Deploy-on-your-server}

Para testar esse recurso no servidor, siga as instruções abaixo:

* [Baixe e extraia o conteúdo do arquivo zip no sistema de arquivos](assets/mult-records-template-and-xml-file.zip)Esse arquivo zip contém o modelo e o arquivo de dados xml.
* [Aponte seu navegador para o Felix web console](http://localhost:4502/system/console/bundles)
* [Implantar o pacote DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Implantar pacote AEMFormsDocumentServices personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)Pacote personalizado que gera os pdf usando a API OutputService
* [Aponte seu navegador para o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Importar e instalar o pacote](assets/generate-multiple-pdf-from-xml.zip). Este pacote contém a página html que permite que você solte o modelo e os arquivos de dados.
* [Aponte seu navegador para MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Arraste e solte o modelo e o arquivo de dados xml juntos
* Baixe o arquivo zip criado. Esse arquivo zip contém os arquivos pdf gerados pelo serviço de saída.

>[!NOTE]
>Há várias maneiras de acionar esse recurso. Neste exemplo, usamos uma interface da Web para descartar o modelo e o arquivo de dados para demonstrar o recurso.
