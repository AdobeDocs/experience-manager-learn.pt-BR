---
title: Uso de fragmentos no serviço de saída
description: Gerar documentos pdf com fragmentos residentes no repositório crx
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 106
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# Geração de documentos pdf usando fragmentos{#developing-with-output-and-forms-services-in-aem-forms}


Neste artigo, usaremos o serviço de saída para gerar arquivos pdf usando fragmentos xdp. O xdp principal e os fragmentos residem no repositório crx. É importante imitar a estrutura de pastas do sistema de arquivos no AEM. Por exemplo, se estiver usando um fragmento na pasta de fragmentos do xdp, você deve criar uma pasta chamada **fragmentos** na pasta base do AEM. A pasta base conterá o modelo base xdp. Por exemplo, se você tiver a seguinte estrutura em seu sistema de arquivos
* c:\xdptemplates - Conterá o modelo base xdp
* c:\xdptemplates\fragments - Essa pasta conterá fragmentos e o modelo principal fará referência ao fragmento conforme mostrado abaixo
  ![fragment-xdp](assets/survey-fragment.png).
* A pasta xdpdocuments conterá o modelo base e os fragmentos na pasta **fragmentos**

Você pode criar a estrutura necessária usando os [formulários e a interface do usuário do documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

A seguir está a estrutura de pastas da amostra xdp que usa 2 fragmentos
![formulários&amp;documento](assets/fragment-folder-structure-ui.png)


* Serviço de saída - Normalmente, esse serviço é usado para mesclar dados xml com modelo xdp ou pdf para gerar pdf nivelado. Para obter mais detalhes, consulte o [javadoc](https://helpx.adobe.com/br/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para o Serviço de saída. Nesta amostra, estamos usando fragmentos que residem no repositório crx.


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

**Para testar o pacote de exemplo em seu sistema**

* [Baixe e importe os arquivos xdp de amostra para o AEM](assets/xdp-templates-fragments.zip)
* [Baixe e instale o pacote usando o gerenciador de pacotes da AEM](assets/using-fragments-assets.zip)
* [A amostra xdp e os fragmentos podem ser baixados aqui](assets/xdptemplates.zip)

incluir na lista de permissões **Depois de instalar o pacote, você terá que copiar as seguintes URLs no Filtro CSRF do Adobe Granite.**

1. Siga as etapas mencionadas abaixo para incluir na lista de permissões os caminhos mencionados acima.
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /content/AemFormsSamples/usingfragments

Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite fazer solicitações POST no servidor. Instale o aplicativo Postman no sistema.
Inicie o aplicativo e insira o seguinte URL para testar a API de dados de exportação

Verifique se você selecionou &quot;POST&quot; na lista suspensa
http://localhost:4502/content/AemFormsSamples/usingfragments.html
Certifique-se de especificar &quot;Autorização&quot; como &quot;Autenticação básica&quot;. Especifique o nome de usuário e a senha do AEM Server
Navegue até a guia &quot;Corpo&quot; e especifique os parâmetros da solicitação, conforme mostrado na imagem abaixo
![exportar](assets/using-fragment-postman.png)
Clique no botão Send

[Você poderia importar essa coleção do carteiro para testar a API](assets/usingfragments.postman_collection.json)
