---
title: Serviços úteis de utilidade pública
description: Alguns serviços de utilitários úteis para desenvolvedores do AEM Forms
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Serviços úteis de utilidade pública

Este pacote de exemplo fornece serviços de utilitários úteis que podem ser usados por um desenvolvedor do AEM Forms. Os serviços a seguir estão disponíveis.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

O pacote de exemplo pode ser [baixado daqui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Código de exemplo para usar os serviços de utilitários

O código a seguir foi usado na página JSP para criar org.w3c.dom.Document a partir de uma sequência de caracteres, converter o documento e armazená-lo no repositório do CRX como mostrado no seguinte fragmento de código.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Pré-requisitos


Será necessário implantar [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar?lang=pt-BR) e iniciar o pacote.


Se você for salvar documentos no repositório do CRX usando este serviço utilitário, siga o [artigo sobre desenvolvimento com usuário de serviço](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=pt-BR#adaptive-forms). Forneça as [permissões necessárias](http://localhost:4502/useradmin) nas pastas apropriadas do CRX para o usuário do serviço fd.
