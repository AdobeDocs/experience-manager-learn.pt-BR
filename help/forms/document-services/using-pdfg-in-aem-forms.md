---
title: Uso do PDFG em AEM Forms
seo-title: Uso do PDFG em AEM Forms
description: Demonstração do recurso de arrastar e soltar para criar PDF usando o AEM Forms
seo-description: Demonstração do recurso de arrastar e soltar para criar PDF usando o AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: gerador de pdf
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 2%

---


# Uso do PDFG no AEM Forms{#using-pdfg-in-aem-forms}

Demonstração do recurso de arrastar e soltar para criar PDF usando o AEM Forms

PDFG significa geração de PDF. Isso significa que é possível converter uma variedade de formatos de arquivo em PDF. Os mais comuns são documentos do Microsoft Office. O PDFG faz parte do AEM Forms desde a versão 6.1.
[O javadoc para a API PDFG está listado aqui](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Os ativos associados a este artigo permitirão arrastar e soltar documentos do MS office ou arquivo JPG para a área designada da página HTML. Depois que o documento for descartado, ele chamará o serviço PDFG e converterá o documento em PDF e salvá-lo no sistema de arquivos do AEM Server.

Para instalar os ativos de demonstração, execute as seguintes etapas

1. Configure o PDFG conforme mencionado neste documento [aqui](https://helpx.adobe.com/br/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Siga a documentação apropriada relacionada à sua versão do AEM Forms.
1. [Importe e instale ativos relacionados a este artigo usando o gerenciador de pacotes.](assets/createpdfgdemov2.zip)
1. [Navegue até post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin no seu CRX
1. Altere o local de salvamento de acordo com sua preferência (linha 9)
1. Salve as alterações.
1. Abra a [ página html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) para arrastar e soltar arquivos para conversão.
1. Solte um arquivo de palavras ou jpg na área de soltar.
1. O documento de entrada será convertido em PDF e salvo no mesmo local especificado no ponto 4.

O trecho de código a seguir mostra o uso do serviço PDFG para converter arquivos em PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

