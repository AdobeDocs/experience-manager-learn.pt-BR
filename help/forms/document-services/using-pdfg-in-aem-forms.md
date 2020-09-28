---
title: Uso do PDFG no AEM Forms
seo-title: Uso do PDFG no AEM Forms
description: Demonstração do recurso Arrastar e soltar para criar PDF usando o AEM Forms
seo-description: Demonstração do recurso Arrastar e soltar para criar PDF usando o AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# Uso do PDFG no AEM Forms{#using-pdfg-in-aem-forms}

Demonstração do recurso Arrastar e soltar para criar PDF usando o AEM Forms

PDFG significa Geração de PDF. Isso significa que você pode converter diversos formatos de arquivo em PDF. Os mais comuns são os documentos do Microsoft Office. O PDFG faz parte do AEM Forms desde 6.1.
[A API javadoc para PDFG está listada aqui](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Os ativos associados a este artigo permitirão que você arraste e solte documentos do MS Office ou arquivos JPG na zona para soltar da página HTML. Quando o documento for solto, ele chamará o serviço PDFG e converterá o documento em PDF e o salvará no sistema de arquivos AEM Server.

Para instalar os ativos de demonstração, execute as seguintes etapas

1. Configure o PDFG conforme mencionado neste documento [aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Siga a documentação apropriada relacionada à sua versão do AEM Forms.
1. [Importe e instale ativos relacionados a este artigo usando o gerenciador de pacotes.](assets/createpdfgdemov2.zip)
1. [Navegue até post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) em seu CRX
1. Altere o local de salvamento de acordo com sua preferência (linha 9)
1. Salve as alterações.
1. Abra a página [](http://localhost:4502/content/DocumentServices/CreatePDFG.html) html para arrastar e soltar arquivos para conversão.
1. Solte um arquivo de palavras ou jpg na área de soltar.
1. O documento de entrada será convertido em PDF e salvo no mesmo local especificado no ponto 4.

O trecho de código a seguir mostra o uso do serviço PDFG para converter arquivos em PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

