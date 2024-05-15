---
title: Serviços úteis de utilidade pública
description: Alguns serviços de utilitários úteis para desenvolvedores do AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

O pacote de amostra pode ser [baixado aqui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Código de exemplo para usar os serviços de utilitários

O código a seguir foi usado na página JSP para criar org.w3c.dom.Document a partir de uma sequência de caracteres, converter o documento e armazená-lo no repositório CRX como mostrado no seguinte fragmento de código.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Pré-requisitos


Será necessário implantar [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e inicie o pacote.


Se você for salvar documentos no repositório CRX usando esses serviços utilitários, siga o [desenvolvendo com o artigo de usuário de serviço](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Certifique-se de fornecer a [permissões necessárias](http://localhost:4502/useradmin) nas pastas CRX apropriadas para o usuário do serviço fd.
