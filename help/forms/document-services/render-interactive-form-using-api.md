---
title: Renderização do PDF interativo usando os serviços da Forms no AEM Forms
description: Usar a API do Forms Service no AEM Forms para renderizar o PDF interativo
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Renderização do PDF interativo usando os serviços da Forms no AEM Forms

Usar a API do Forms Service no AEM Forms para renderizar o PDF interativo

Neste artigo, analisaremos o seguinte serviço

* FormsService - Esse é um serviço muito versátil que permite exportar/importar dados de e para o arquivo PDF e também gerar pdf interativo ao mesclar dados xml no modelo xdp

O funcionário [javadoc para API do AEM Forms está listado aqui](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

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
1. [Baixe e instale o DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Baixe e instale o Pacote de amostra do DocumentServices usando o Felix Web Console](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Baixe e instale o pacote usando o gerenciador de pacotes de AEM](assets/downloadinteractivepdffrommobileform.zip)

1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procure por Filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /bin/generateinteractivepdf
1. Procurar por _Serviço Mapeador de Usuário do Apache Sling Service_ e clique em para abrir as propriedades
   1. Clique no botão *+* ícone (mais) para adicionar o seguinte Mapeamento de serviço
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Clique em &#39; Salvar &#39;
1. [Abrir o formulário móvel](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Preencha alguns campos e clique no botão ***Baixar e preencher ....*** botão
1. O pdf interativo deve ser baixado no sistema local


O pacote de amostra contém o perfil personalizado associado ao Formulário móvel. Explore [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) arquivo. Esse jsp extrai os dados do formulário móvel e faz uma solicitação de POST para o servlet montado em ***/bin/generateinteractivepdf*** caminho. O servlet retorna o pdf interativo para o aplicativo que faz a chamada. O código no customtoolbar.jsp então baixa o arquivo no sistema local
