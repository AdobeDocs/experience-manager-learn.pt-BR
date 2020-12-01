---
title: Desenvolvimento com serviços de saída e Forms na AEM Forms
seo-title: Desenvolvimento com serviços de saída e Forms na AEM Forms
description: Usando a Saída e a API de serviço da Forms no AEM Forms
seo-description: Usando a Saída e a API de serviço da Forms no AEM Forms
feature: forms-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---


# Como renderizar PDF interativo usando os serviços Forms no AEM Forms

Uso da API de serviço da Forms no AEM Forms para renderizar PDF interativo

Neste artigo, vamos verificar o seguinte serviço

* FormsService - este é um serviço muito versátil que permite exportar/importar dados de e para o arquivo PDF e também gerar pdf interativo ao unir dados xml ao modelo xdp

O javadoc oficial da API AEM Forms está listado [aqui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

O trecho de código a seguir renderiza o pdf interativo usando a operação renderPDFForm do FormsService. O schAccord.xdp é um modelo que está sendo usado para unir os dados xml.

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

Linha 2-4: Criar PDFFormRenderOptions e definir suas propriedades

Linha 7: Gerar PDF interativo usando a operação de serviço renderPDFForm do FormsService

Linha 11: Retorna o pdf interativo gerado para o aplicativo chamador

**Para testar o pacote de amostra em seu sistema**
1. [Baixe e instale o Pacote de amostra do DocumentServices usando o Console Web do Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Baixe e instale o pacote usando o gerenciador de pacote AEM](assets/downloadinteractivepdffrommobileform.zip)



1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procurar o Filtro CSRF Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /bin/generateinterativepdf
1. [Abrir o formulário móvel](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Preencha alguns campos e clique em ***Baixar e preencher ....*** botão
1. O pdf interativo deve ser baixado no sistema local


O pacote de amostra contém o perfil personalizado associado ao Formulário móvel. Explore o arquivo [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Esse jsp extrai os dados do formulário móvel e faz uma solicitação POST para o servlet montado no caminho ***/bin/generateinterativepdf***. O servlet retorna o pdf interativo para o aplicativo de chamada. O código no customtoolbar.jsp, em seguida, baixa o arquivo no sistema local


