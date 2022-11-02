---
title: Uso de fragmentos no serviço de saída com a pasta assistida
description: Gere documentos pdf com fragmentos residentes no repositório crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Geração de documento pdf com fragmentos usando script ECMA{#developing-with-output-and-forms-services-in-aem-forms}


Neste artigo, usaremos o serviço de saída para gerar arquivos pdf usando fragmentos xdp. O xdp principal e os fragmentos residem no repositório crx. É importante imitar a estrutura de pastas do sistema de arquivos no AEM. Por exemplo, se você estiver usando um fragmento na pasta de fragmentos em seu xdp, deverá criar uma pasta chamada **fragmentos** em sua pasta base no AEM. A pasta base conterá seu template xdp base. Por exemplo, se você tiver a seguinte estrutura em seu sistema de arquivos
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* A pasta xdpdocuments conterá seu modelo base e os fragmentos em **fragmentos** pasta

Você pode criar a estrutura necessária usando o [interface do usuário de formulários e documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Esta é a estrutura de pastas para o xdp de amostra que usa 2 fragmentos
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Serviço de saída - Normalmente, esse serviço é usado para unir dados xml ao modelo xdp ou pdf para gerar pdf nivelado. Para obter mais detalhes, consulte o [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para o serviço de saída. Nessa amostra, estamos usando fragmentos que residem no repositório crx.


O script ECMA a seguir foi usado para gerar PDF. Observe o uso de ResourceResolver e ResourceResolverHelper no código. O ResourceReolver é necessário, pois esse código está sendo executado fora de qualquer contexto de usuário.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**Para testar o pacote de amostra em seu sistema**
* [Implante o pacote DevelopingWithServiceUSer](assets/DevelopingWithServiceUser.jar)
* Adicionar a entrada **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** na alteração de serviço do mapeador de usuários, conforme mostrado na captura de tela abaixo
   ![alteração do mapa do usuário](assets/user-mapper-service-amendment.png)
* [Baixe e importe arquivos xdp de amostra e scripts ECMA](assets/watched-folder-fragments-ecma.zip).
Isso criará uma estrutura de pastas assistida na pasta c:/fragmentsandoutputservice

* [Extrair o arquivo de dados de amostra](assets/usingFragmentsSampleData.zip) e coloque-o na pasta de instalação da sua pasta assistida (c:\fragmentsandoutputservice\install)

* Verifique a pasta de resultados da sua configuração de pasta assistida para o arquivo pdf gerado
