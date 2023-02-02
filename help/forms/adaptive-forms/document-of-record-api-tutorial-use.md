---
title: Usando a API para gerar o Documento de registro com o AEM Forms
description: Gerar Documento de Registro (DOR) programaticamente
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2023-01-26T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---

# Usar API para gerar documento de registro no AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Gerar Documento de Registro (DOR) programaticamente

Este artigo ilustra o uso da variável `com.adobe.aemds.guide.addon.dor.DoRService API` para gerar **Documento de registro** programaticamente. [Documento de registro](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) é uma versão PDF dos dados capturados no formulário adaptável.

1. Veja a seguir o trecho de código. A primeira linha recebe o serviço DOR.
1. Defina as opções do.
1. Chame o método de renderização do DoRService e transmita o objeto DoROoptions para o método de renderização

```java
String dataXml = request.getParameter("data");
System.out.println("Got " + dataXml);
Session session;
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
System.out.println("Got ... DOR Service");
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
System.out.println("Got aem DemoListings");
resourceResolver = aemDemoListings.getFormsServiceResolver();
session = resourceResolver.adaptTo(Session.class);
resource = resourceResolver.getResource("/content/forms/af/sandbox/1201-borrower-payments");
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions = new com.adobe.aemds.guide.addon.dor.DoROptions();
dorOptions.setData(dataXml);
dorOptions.setFormResource(resource);
java.util.Locale locale = new java.util.Locale("en");
dorOptions.setLocale(locale);
com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
byte[] fileBytes = dorResult.getContent();
com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
resource = resourceResolver.getResource("/content/usergenerated/content/aemformsenablement");
Node paydotgov = resource.adaptTo(Node.class);
java.util.Random r = new java.util.Random();
String nodeName = Long.toString(Math.abs(r.nextLong()), 36);
Node fileNode = paydotgov.addNode(nodeName + ".pdf", "nt:file");

System.out.println("Created file Node...." + fileNode.getPath());
Node contentNode = fileNode.addNode("jcr:content", "nt:resource");
Binary binary = session.getValueFactory().createBinary(dorDocument.getInputStream());
contentNode.setProperty("jcr:data", binary);
JSONWriter writer = new JSONWriter(response.getWriter());
writer.object();
writer.key("filePath");
writer.value(fileNode.getPath());
writer.endObject();
session.save();
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
1. Certifique-se de iniciar AEM servidor como um administrador (pelo menos no Windows)
1. Verifique se o pacote &quot;DevelopingWithServiceUser&quot; está em *estado ativo*
1. [Certifique-se de que o usuário do sistema](http://localhost:4502/useradmin) &#39; fd-service&#39; tem permissões de Leitura, Modificação e Criação no seguinte nó `/content/usergenerated/content/aemformsenablement`
