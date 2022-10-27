---
title: Usando a API para gerar o Documento de registro com o AEM Forms
description: Gerar Documento de Registro (DOR) programaticamente
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 1%

---

# Usar API para gerar documento de registro no AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Gerar Documento de Registro (DOR) programaticamente

Este artigo ilustra o uso da variável `com.adobe.aemds.guide.addon.dor.DoRService API` para gerar **Documento de registro** programaticamente. [Documento de registro](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) é uma versão PDF dos dados capturados no formulário adaptável.

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
1. Certifique-se de ter instalado e iniciado o pacote DevelopingWithServiceUser fornecido como parte do [Artigo Criar Usuário do Serviço](service-user-tutorial-develop.md)
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procurar Serviço de Mapeador de Usuários do Apache Sling Service
1. Certifique-se de que a seguinte entrada _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ na seção Service Mappings
1. [Abra o formulário](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Preencha o formulário e clique em &quot; Ver PDF &quot;
1. Você deve ver o DOR na nova guia no navegador


**Dicas de solução de problemas**

O PDF não é exibido na nova guia do navegador:

1. Certifique-se de que você não está bloqueando pop-ups no seu navegador
1. Siga as etapas descritas neste [artigo](service-user-tutorial-develop.md)
1. Verifique se o pacote &quot;DevelopingWithServiceUser&quot; está em *estado ativo*
1. Certifique-se de que o usuário do sistema &#39; data &#39; tenha permissões de Leitura, Modificação e Criação no seguinte nó `/content/usergenerated/content/aemformsenablement`
