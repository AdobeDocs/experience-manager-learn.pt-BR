---
title: Usar a API para gerar o documento de registro com o AEM Forms
seo-title: Usar a API para gerar o documento de registro com o AEM Forms
description: Gerar Documento de Registro (DOR) programaticamente
seo-description: Usar a API para gerar o documento de registro com o AEM Forms
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Usando a API para gerar o Documento de registro nos AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Gerar Documento de Registro (DOR) programaticamente

Este artigo ilustra o uso do `com.adobe.aemds.guide.addon.dor.DoRService API` para gerar **Documento de registro** programaticamente. [Documento de ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) registro é uma versão PDF dos dados capturados no formulário adaptável.

1. Veja a seguir o trecho de código. A primeira linha recebe o serviço DOR.
1. Defina as opções do.
1. Chame o método de renderização do DoRService e transmita o objeto DoROoptions para o método de renderização

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

Para experimentar isso no sistema local, siga as seguintes etapas

1. [Baixe e instale os ativos do artigo usando o gerenciador de pacotes](assets/dor-with-api.zip)
1. Certifique-se de ter instalado e iniciado o pacote DevelopingWithServiceUser fornecido como parte de [Criar Usuário do Serviço artigo](service-user-tutorial-develop.md)
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procurar Serviço de Mapeador de Usuários do Apache Sling Service
1. Certifique-se de inserir a seguinte entrada _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ na seção Mapeamentos de serviço
1. [Abra o formulário](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Preencha o formulário e clique em &#39; Exibir PDF &#39;
1. Você deve ver o DOR na nova guia no navegador


**Dicas de solução de problemas**

O PDF não é exibido na nova guia do navegador:

1. Certifique-se de que você não está bloqueando pop-ups no seu navegador
1. Siga as etapas descritas neste [artigo](service-user-tutorial-develop.md)
1. Certifique-se de que o pacote &#39;DevelopingWithServiceUser&#39; está no *estado ativo*
1. Certifique-se de que o usuário do sistema &#39; data &#39; tenha permissões Ler, Modificar e Criar no seguinte nó `/content/usergenerated/content/aemformsenablement`

