---
title: Combine arquivos de PDF usando a operação de invocar DDX
description: Faça uma solicitação POST para invocar o endpoint do DDX com os parâmetros necessários
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 693dac88-84f3-4051-8e46-3105093711a3
duration: 50
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 0%

---

# Efetuar a chamada POST


A próxima etapa é fazer uma chamada de POST HTTP para o endpoint com os parâmetros necessários. O DDX e os arquivos pdf são fornecidos como arquivos de recurso. O ponto de extremidade tem autenticação baseada em token; passamos o token de acesso no cabeçalho da solicitação.
Ao usar o serviço Assembler, use uma linguagem baseada em XML chamada Document Description XML (DDX) para descrever a saída desejada. O DDX é uma linguagem de marcação declarativa cujos elementos representam os blocos de construção de documentos. O DDX a seguir foi usado para mesclar os dois documentos PDF identificados nos elementos de origem do PDF.

```xml
<DDX xmlns="http://ns.adobe.com/DDX/1.0/">
<PDF result="doc3.pdf"> 
	<PDF source="CA-Drivers-Handbook.pdf"/>
 	<PDF source="CA-Parent-Teen-Handbook.pdf"/>
  </PDF>
</DDX>
```

O código a seguir foi usado para combinar arquivos pdf

```java
package com.aemformscs.documentservices;

import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;

import org.apache.commons.io.IOUtils;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.HttpMultipartMode;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;

public class AssemblePDFFiles {
  public String SAVE_LOCATION = "c:\\assembledpdf";

  public void assemblePDF(String postURL) {

    HttpPost httpPost = new HttpPost(postURL);
    CredentialUtilites cu = new CredentialUtilites();
    String accessToken = cu.getAccessToken();
    httpPost.addHeader("Authorization", "Bearer " + accessToken);
    ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
    URL ddxFile = classLoader.getResource("ddxfiles/assemble2pdfs.ddx");
    MultipartEntityBuilder builder = MultipartEntityBuilder.create();

    builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

    File ddx = new File(ddxFile.getPath());
    builder.addBinaryBody("ddx", ddx, ContentType.create("application/xml"), ddx.getName());
    URL url = classLoader.getResource("pdffiles");
    System.out.println(url.getPath());
    File files[] = new File(url.getPath()).listFiles();

    for (int i = 0; i < files.length; i++) {
      System.out.println("Added  " + files[i].getName());
      builder.addBinaryBody(files[i].getName(), files[i], ContentType.APPLICATION_OCTET_STREAM, files[i].getName());
    }
    try {

      HttpEntity entity = builder.build();
      httpPost.setEntity(entity);
      CloseableHttpClient httpclient = HttpClients.createDefault();
      CloseableHttpResponse response = httpclient.execute(httpPost);
      System.out.println("The success code is " + response.getStatusLine().getStatusCode());
      InputStream generatedPDF = response.getEntity().getContent();
      byte[] bytes = IOUtils.toByteArray(generatedPDF);
      File saveLocation = new File(SAVE_LOCATION);
      if (!saveLocation.exists()) {
        saveLocation.mkdirs();
      }
      File outputFile = new File(SAVE_LOCATION + File.separator + "assembledPDF.pdf");
      FileOutputStream outputStream = new FileOutputStream(outputFile);
      outputStream.write(bytes);
      outputStream.close();
      System.out.println("AssembledPDF.pdf saved to " + SAVE_LOCATION);

    } catch (Exception e) {
      System.out.println("The message is " + e.getMessage());
    }
  }

}
```
