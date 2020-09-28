---
title: Usar a API para gerar o Documento de Registro com a AEM Forms
seo-title: Usar a API para gerar o Documento de Registro com a AEM Forms
description: Gerar Documento de registro (DOR) programaticamente
seo-description: Usar a API para gerar o Documento de Registro com a AEM Forms
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Usar a API para gerar o Documento de registro no AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Gerar Documento de registro (DOR) programaticamente

Este artigo ilustra o uso do `com.adobe.aemds.guide.addon.dor.DoRService API` para gerar **Documento de Registro** de forma programática. [Documento de registro](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) é uma versão PDF dos dados capturados no formulário adaptável.

1. A seguir está o trecho de código. A primeira linha recebe o serviço DOR.
1. Defina o DoRO.
1. Chame o método de renderização do DoRService e passe o objeto DoROoptions para o método de renderização

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

Para experimentar isso no sistema local, siga as etapas a seguir

1. [Baixe e instale os ativos do artigo usando o gerenciador de pacotes](assets/dor-with-api.zip)
1. Verifique se você instalou e iniciou o pacote DevelopingWithServiceUser fornecido como parte do artigo [Create Service User (Criar usuário do serviço)](service-user-tutorial-develop.md)
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procurar o serviço Mapeador de Usuário do Apache Sling Service
1. Certifique-se de que você tenha a seguinte entrada _DevelopingWithServiceUser.core:getformsresouresolver=fd-service_ na seção Service Mappings
1. [Abrir o formulário](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Preencha o formulário e clique em &#39; PDF de Visualização &#39;
1. Você deve ver o DOR na nova guia no navegador


**Dicas para solução de problemas**

O PDF não é exibido na nova guia do navegador:

1. Verifique se você não está bloqueando pop-ups no navegador
1. Faça com que você siga as etapas descritas neste [artigo](service-user-tutorial-develop.md)
1. Verifique se o pacote &#39;DevelopingWithServiceUser&#39; está no estado *ativo*
1. Verifique se os dados do usuário do sistema &#39; têm permissões de Leitura, Modificação e Criação no nó a seguir `/content/usergenerated/content/aemformsenablement`

