---
title: Renderização do PDF interativo usando os serviços da Forms no AEM Forms
description: Utilização da API de serviço do Forms no AEM Forms para renderizar o PDF interativo
feature: Forms Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Renderização do PDF interativo usando os serviços da Forms no AEM Forms

Utilização da API de serviço do Forms no AEM Forms para renderizar o PDF interativo

Neste artigo, analisaremos o seguinte serviço

* FormsService — é um serviço muito versátil que permite exportar/importar dados de e para um arquivo do PDF e também gerar PDF interativo mesclando dados xml com um modelo xdp

O [javadoc oficial para a API do AEM Forms está listado aqui](https://helpx.adobe.com/br/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

O trecho de código a seguir renderiza o pdf interativo usando a operação renderPDFForm do FormsService. O schengen.xdp é o modelo que está sendo usado para mesclar os dados xml.

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

Linha 1: local da pasta que contém o modelo xdp

Linha 2-4: Criar PDFFormRenderOptions e definir suas propriedades

Linha 7: gerar o PDF interativo usando a operação de serviço renderPDFForm do FormsService

Linha 11: retorna o pdf interativo gerado para o aplicativo de chamada

**Para testar o pacote de exemplo em seu sistema**
1. [Baixe e instale o DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Baixe e instale o Pacote de amostra de serviços de documento usando o Felix Web Console](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Baixe e instale o pacote usando o gerenciador de pacotes da AEM](assets/downloadinteractivepdffrommobileform.zip)

1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve
1. /bin/generateinteractivepdf
1. Pesquise por _Serviço Mapeador de Usuários do Apache Sling Service_ e clique em para abrir as propriedades
   1. Clique no ícone *+* (mais) para adicionar o seguinte Service Mapping
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Clique em &quot;Salvar&quot;
1. [Abrir o formulário móvel](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Preencha alguns campos e clique em ***Baixar e preencher ....Botão***
1. O pdf interativo deve ser baixado no sistema local


O pacote de exemplo contém o perfil personalizado associado ao Formulário para dispositivos móveis. Explore o arquivo [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Este jsp extrai os dados do formulário para publicação de conteúdo para dispositivos móveis e faz uma solicitação POST para o servlet montado no caminho ***/bin/generateinteractivepdf***. O servlet retorna o pdf interativo para o aplicativo chamador. O código no customtoolbar.jsp faz o download do arquivo para o seu sistema local
