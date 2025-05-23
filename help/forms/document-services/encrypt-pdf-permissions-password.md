---
title: Criptografar o PDF com uma senha de permissões
description: Usar DocAssuranceService para criptografar um PDF
feature: Document Services
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Criptografar o PDF com uma senha de permissão

Uma senha de permissões, também conhecida como proprietário ou senha mestre, é necessária para copiar, editar ou imprimir um documento do PDF. Saiba como usar a API [DocAssuranceService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) para aplicar uma senha de permissão a um PDF de forma programática.

O código JSP a seguir criptografa um PDF com uma senha de permissões:

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**Para testar o pacote de exemplo em seu sistema**

[Baixe e instale o pacote usando o gerenciador de pacotes da AEM](assets/encryptpdf.zip)

**instale o pacote, adicione as seguintes URLs na configuração OSGi do Filtro CSRF do Adobe Granite Depois de incluir na lista de permissões:**

1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /content/AemFormsSamples/encrypt

## Teste da amostra

Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite fazer solicitações POST no servidor. A captura de tela a seguir mostra os parâmetros de solicitação necessários para que a solicitação de publicação funcione. Certifique-se de especificar o tipo de autorização apropriado antes de enviar a solicitação.

![criptografar-pdf-postman](assets/encrypt-pdf-postman.png)
