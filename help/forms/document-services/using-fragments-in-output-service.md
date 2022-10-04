---
title: Uso de fragmentos no serviço de saída
description: Gere documentos pdf com fragmentos residentes no repositório crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
source-git-commit: 747d1823ce1bc6670d1e80abcf6483ac921c0a01
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 1%

---

# Geração de documentos pdf usando fragmentos{#developing-with-output-and-forms-services-in-aem-forms}


Neste artigo, usaremos o serviço de saída para gerar arquivos pdf usando fragmentos xdp. O xdp principal e os fragmentos residem no repositório crx. É importante imitar a estrutura de pastas do sistema de arquivos no AEM. Por exemplo, se você estiver usando um fragmento na pasta de fragmentos em seu xdp, deverá criar uma pasta chamada **fragmentos** em sua pasta base no AEM. A pasta base conterá seu template xdp base. Por exemplo, se você tiver a seguinte estrutura em seu sistema de arquivos
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* A pasta xdpdocuments conterá seu modelo base e os fragmentos em **fragmentos** pasta

Você pode criar a estrutura necessária usando o [interface do usuário de formulários e documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Esta é a estrutura de pastas para o xdp de amostra que usa 2 fragmentos
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Serviço de saída - Normalmente, esse serviço é usado para unir dados xml ao modelo xdp ou pdf para gerar pdf nivelado. Para obter mais detalhes, consulte o [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para o serviço de saída. Nessa amostra, estamos usando fragmentos que residem no repositório crx.


O código a seguir foi usado para incluir fragmentos no arquivo PDF

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**Para testar o pacote de amostra em seu sistema**

* [Baixe e importe arquivos xdp de amostra para o AEM](assets/xdp-templates-fragments.zip)
* [Baixe e instale o pacote usando o gerenciador de pacotes de AEM](assets/using-fragments-assets.zip)
* [A amostra de xdp e fragmentos pode ser baixada aqui](assets/xdptemplates.zip)

**Depois de instalar o pacote, você terá que lista de permissões os seguintes URLs no Adobe Granite CSRF Filter.**

1. Siga os passos mencionados abaixo para lista de permissões os caminhos mencionados acima.
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procure por Filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /content/AemFormsSamples/uso de fragmentos

Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite fazer solicitações do POST para o seu servidor. Instale o aplicativo Postman em seu sistema.
Inicie o aplicativo e insira o seguinte URL para testar a API de dados de exportação

Selecione &quot;POST&quot; na lista suspensa http://localhost:4502/content/AemFormsSamples/usingfragments.html Certifique-se de especificar &quot;Autorização&quot; como &quot;Autenticação básica&quot;. Especifique o nome de usuário e a senha do AEM Server Navegue até a guia &quot;Corpo&quot; e especifique os parâmetros da solicitação, conforme mostrado na imagem abaixo
![exportar](assets/using-fragment-postman.png)
Em seguida, clique no botão Send

[É possível importar essa coleção de cartazes para testar a API](assets/usingfragments.postman_collection.json)
