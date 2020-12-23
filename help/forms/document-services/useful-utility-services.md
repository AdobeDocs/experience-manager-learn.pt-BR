---
title: Serviços de utilidade pública
description: Alguns serviços utilitários úteis para desenvolvedores AEM Forms
feature: document-services
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 243e26e2403e3d7816a0dd024ffbe73743827c7c
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---


# Serviços de utilidade pública

Este pacote de amostra fornece serviços utilitários úteis que podem ser usados por um desenvolvedor AEM Forms. Os seguintes serviços estão disponíveis.


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

O pacote de amostra pode ser [baixado daqui](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Exemplo de código para usar os serviços de utilidade pública

A seguir está o código que foi usado na página JSP para criar org.w3c.dom.Documento a partir da sequência de caracteres e converter o documento e armazená-lo no repositório CRX, conforme mostrado no trecho de código a seguir.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Pré-requisitos


Você precisará implantar [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e start o pacote.


Se você for salvar documentos no repositório CRX usando esse serviço de utilitários, siga o [artigo de desenvolvimento com o usuário do serviço](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Certifique-se de fornecer as [permissões necessárias](http://localhost:4502/useradmin) nas pastas CRX apropriadas ao usuário fd-service.

