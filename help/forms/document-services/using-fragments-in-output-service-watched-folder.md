---
title: Uso de fragmentos no serviço de saída com a pasta monitorada
description: Gerar documentos pdf com fragmentos residentes no repositório crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
duration: 120
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# Geração de documento pdf com fragmentos usando o script ECMA{#developing-with-output-and-forms-services-in-aem-forms}


Neste artigo, usaremos o serviço de saída para gerar arquivos pdf usando fragmentos xdp. O xdp principal e os fragmentos residem no repositório crx. É importante imitar a estrutura de pastas do sistema de arquivos no AEM. Por exemplo, se estiver usando um fragmento na pasta de fragmentos do xdp, você deve criar uma pasta chamada **fragmentos** em sua pasta base no AEM. A pasta base conterá o modelo base xdp. Por exemplo, se você tiver a seguinte estrutura em seu sistema de arquivos
* c:\xdptemplates - Conterá o modelo base xdp
* c:\xdptemplates\fragments - Essa pasta conterá fragmentos e o modelo principal fará referência ao fragmento conforme mostrado abaixo
  ![fragment-xdp](assets/survey-fragment.png).
* A pasta xdpdocuments conterá o modelo base e os fragmentos em **fragmentos** pasta

É possível criar a estrutura necessária usando o [formulários e interface do usuário do documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

A seguir está a estrutura de pastas da amostra xdp que usa 2 fragmentos
![formulários&amp;documento](assets/fragment-folder-structure-ui.png)


* Serviço de saída - Normalmente, esse serviço é usado para mesclar dados xml com modelo xdp ou pdf para gerar pdf nivelado. Para obter mais detalhes, consulte [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) para o Serviço de saída. Nesta amostra, estamos usando fragmentos que residem no repositório crx.


O script ECMA a seguir foi usado para gerar PDF. Observe o uso de ResourceResolver e ResourceResolverHelper no código. O ResourceResolver é necessário, pois esse código está sendo executado fora de qualquer contexto de usuário.

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

**Para testar o pacote de amostra no seu sistema**
* [Implantar o pacote DevelopingWithServiceUSer](assets/DevelopingWithServiceUser.jar)
* Adicionar a entrada **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** na emenda do serviço mapeador do usuário, conforme mostrado na captura de tela abaixo
  ![alteração do mapeador de usuários](assets/user-mapper-service-amendment.png)
* [Baixe e importe os arquivos xdp de amostra e os scripts ECMA](assets/watched-folder-fragments-ecma.zip).
Isso criará uma estrutura de pasta monitorada na pasta c:/fragmentsandoutputservice

* [Extraia o arquivo de dados de amostra](assets/usingFragmentsSampleData.zip) e coloque-o na pasta install da pasta monitorada (c:\fragmentsandoutputservice\install)

* Verifique a pasta de resultados da configuração da pasta monitorada para o arquivo pdf gerado
