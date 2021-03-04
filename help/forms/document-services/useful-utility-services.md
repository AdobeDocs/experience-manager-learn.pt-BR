---
title: Serviços de utilidade pública
description: Alguns serviços úteis de utilitários para desenvolvedores do AEM Forms
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 3%

---


# Serviços de utilidade pública

Este pacote de amostra fornece serviços úteis que podem ser usados por um desenvolvedor do AEM Forms. Os seguintes serviços estão disponíveis.


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

## Código de exemplo para usar o(s) serviço(s) de utilitário(s)

Este é o código que foi usado na página JSP para criar org.w3c.dom.Document a partir da string e converter o documento e armazená-lo no repositório CRX, conforme mostrado no trecho de código a seguir.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Pré-requisitos


Você precisará implantar [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) e iniciar o pacote.


Se você for salvar documentos no repositório CRX usando esse serviço de utilitário, siga o [desenvolvimento com artigo do usuário de serviço](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Certifique-se de fornecer as [permissões necessárias](http://localhost:4502/useradmin) nas pastas CRX apropriadas para o usuário fd-service.

