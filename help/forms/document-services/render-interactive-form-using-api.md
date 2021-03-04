---
title: Desenvolvimento com serviços de saída e formulários no AEM Forms
seo-title: Desenvolvimento com serviços de saída e formulários no AEM Forms
description: Uso da API do Serviço de saída e formulários no AEM Forms
seo-description: Uso da API do Serviço de saída e formulários no AEM Forms
feature: Serviço do Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 1%

---


# Como renderizar PDF interativo usando os serviços do Forms no AEM Forms

Uso da API do Serviço de formulários no AEM Forms para renderizar PDF interativo

Neste artigo, analisaremos o seguinte serviço

* FormsService - Esse é um serviço muito versátil que permite exportar/importar dados de e para o arquivo PDF e também gerar pdf interativo ao mesclar dados xml no modelo xdp

O javadoc oficial da API do AEM Forms é listado [aqui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

O trecho de código a seguir renderiza o pdf interativo usando a operação renderPDFForm do FormsService. O schengen.xdp é um modelo que está sendo usado para unir os dados xml.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

Linha 1: Localização da pasta que contém o modelo xdp

Linha 2-4: Crie PDFFormRenderOptions e defina suas propriedades

Linha 7: Gerar PDF interativo usando a operação de serviço renderPDFForm do FormsService

Linha 11: Retorna o pdf interativo gerado para o aplicativo chamador

**Para testar o pacote de amostra em seu sistema**
1. [Baixe e instale o Pacote de amostra do DocumentServices usando o Felix Web Console](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Baixe e instale o pacote usando o gerenciador de pacotes do AEM](assets/downloadinteractivepdffrommobileform.zip)



1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar o filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /bin/generateinteractivepdf
1. [Abrir o formulário móvel](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Preencha alguns campos e clique em ***Download e preencha ....*** botão
1. O pdf interativo deve ser baixado no sistema local


O pacote de amostra contém o perfil personalizado associado ao Formulário móvel. Explore o arquivo [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Esse jsp extrai os dados do formulário móvel e faz uma solicitação POST para o servlet montado no caminho ***/bin/generateinteractivepdf***. O servlet retorna o pdf interativo para o aplicativo que faz a chamada. O código no customtoolbar.jsp então baixa o arquivo no sistema local


