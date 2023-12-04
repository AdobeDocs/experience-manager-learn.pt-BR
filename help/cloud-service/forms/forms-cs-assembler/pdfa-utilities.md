---
title: Converta PDF em PDF/A.
description: Criar e validar arquivos PDF/A no Forms CA usando os endpoints HTTP
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-10105
exl-id: a4955104-8a87-4add-85c7-c3e3395f5f1a
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 3%

---

# Criar e validar documentos do PDF/A

PDF/A é uma versão padronizada em ISO do Portable Document Format (PDF), especializada para uso no arquivamento e na preservação de documentos eletrônicos a longo prazo. PDF/A difere de PDF proibindo recursos inadequados para arquivamento de longo prazo, como vinculação de fontes (em oposição à incorporação de fontes) e criptografia.

## Converter para PDF/A

O código a seguir foi usado para converter PDF em PDF/A

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
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class PDFAUtilities {
  public String SAVE_LOCATION = "c:\\pdfa";
  public void toPDFA(String postURL) {

    HttpPost httpPost = new HttpPost(postURL);
    CredentialUtilites cu = new CredentialUtilites();
    String accessToken = cu.getAccessToken();
    httpPost.addHeader("Authorization", "Bearer " + accessToken);
    ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
    URL fileToConvert = classLoader.getResource("pdffiles/Address.pdf");
    MultipartEntityBuilder builder = MultipartEntityBuilder.create();

    builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

    File pdfToConvert = new File(fileToConvert.getPath());
    builder.addBinaryBody("inDoc", pdfToConvert, ContentType.create("application/pdf"), pdfToConvert.getName());
    try {

      HttpEntity entity = builder.build();
      httpPost.setEntity(entity);
      CloseableHttpClient httpclient = HttpClients.createDefault();
      CloseableHttpResponse response = httpclient.execute(httpPost);
      if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File saveLocation = new File(SAVE_LOCATION);
        if (!saveLocation.exists()) {
          saveLocation.mkdirs();
        }
        File outputFile = new File(SAVE_LOCATION + File.separator + "pdfa.pdf");
        FileOutputStream outputStream = new FileOutputStream(outputFile);
        outputStream.write(bytes);
        outputStream.close();
        System.out.println("PDF was converted to PDFA and  saved to " + SAVE_LOCATION);

      } else {
        String json_string = EntityUtils.toString(response.getEntity());
        JsonObject responseJson = JsonParser.parseString(json_string).getAsJsonObject();
        System.out.println("Could not convert to PDF/A   - " + responseJson.get("message").getAsString());
      }

    } catch (Exception e) {
      System.out.println("The message is " + e.getMessage());
    }
  }

}
```

## Validar PDF/A

O código a seguir é usado para validar um determinado PDF para conformidade com PDF/A.

```java
public void validatePDFA(String postURL) {

  HttpPost httpPost = new HttpPost(postURL);
  CredentialUtilites cu = new CredentialUtilites();
  String accessToken = cu.getAccessToken();
  httpPost.addHeader("Authorization", "Bearer " + accessToken);
  ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
  URL fileToValidate = classLoader.getResource("pdffiles/pdfa.pdf");
  MultipartEntityBuilder builder = MultipartEntityBuilder.create();

  builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
  builder.addBinaryBody("options", GetOptions.getPDFAOptions().getBytes(), ContentType.APPLICATION_JSON, "options");

  File pdfToValidate = new File(fileToValidate.getPath());
  builder.addBinaryBody("inDoc", pdfToValidate, ContentType.create("application/pdf"), pdfToValidate.getName());
  try {

    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200) {
      String json_string = EntityUtils.toString(response.getEntity());
      JsonObject responseJson = JsonParser.parseString(json_string).getAsJsonObject();
      System.out.println("Th document is    - " + responseJson.toString());

    } else {
      System.out.println(response.getStatusLine().getStatusCode());
    }

  } catch (Exception e) {
    System.out.println("The message is " + e.getMessage());
  }
}
```
