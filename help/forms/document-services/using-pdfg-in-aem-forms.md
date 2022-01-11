---
title: Uso do PDFG no AEM Forms
description: Demonstração do recurso de arrastar e soltar para criar o PDF usando o AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 2%

---

# Uso do PDFG no AEM Forms{#using-pdfg-in-aem-forms}

Demonstração do recurso de arrastar e soltar para criar o PDF usando o AEM Forms

PDFG significa PDF Generation. Isso significa que você pode converter uma variedade de formatos de arquivo em PDF. Os mais comuns são documentos do Microsoft Office. O PDFG faz parte do AEM Forms desde a versão 6.1.
[O javadoc para API PDFG está listado aqui](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

Os ativos associados a este artigo permitirão arrastar e soltar documentos do MS office ou arquivo JPG na área de soltar da página HTML. Depois que o documento for descartado, ele chamará o serviço PDFG e converterá o documento em PDF e salvá-lo no sistema de arquivos AEM Server.

Para instalar os ativos de demonstração, execute as seguintes etapas

1. Configure o PDFG conforme mencionado neste documento [here](https://helpx.adobe.com/br/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Siga a documentação apropriada relacionada à sua versão do AEM Forms.
1. [Importe e instale ativos relacionados a este artigo usando o gerenciador de pacotes.](assets/createpdfgdemov2.zip)
1. [Navegue para post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) em seu CRX
1. Altere o local de salvamento de acordo com sua preferência (linha 9)
1. Salve as alterações.
1. Abra o [  página html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) para arrastar e soltar arquivos para conversão.
1. Solte um arquivo de palavras ou jpg na área de soltar.
1. O documento de entrada será convertido em PDF e guardado no mesmo local especificado no ponto 4.

O trecho de código a seguir mostra o uso do serviço PDFG para converter arquivos no PDF

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
