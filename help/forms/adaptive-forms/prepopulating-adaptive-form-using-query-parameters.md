---
title: Preencha o Adaptive Forms usando parâmetros de consulta.
description: Preencha o Adaptive Forms com dados de parâmetros de consulta.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Preencher previamente o Forms adaptável usando parâmetros de consulta

Um de nossos clientes tinha o requisito de preencher o formulário adaptável usando os parâmetros de consulta. Por exemplo, no url a seguir, os campos Nome e Sobrenome no formulário adaptável são definidos como João e Silva, respectivamente

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Para realizar esse caso de uso, um novo modelo de formulário adaptável foi criado e associado a um componente de página. Neste componente da página, temos um jsp para obter os parâmetros de consulta e criar uma estrutura xml que pode ser usada para preencher o formulário adaptável.

Os detalhes sobre a criação de um novo modelo de formulário adaptável e componente de página são [explicadas neste vídeo.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

Veja a seguir o código que foi usado na página jsp

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>Se o formulário estiver usando um schema, a estrutura do xml será diferente e você terá que criar o xml adequadamente.


## Implantar os ativos no sistema

* [Baixe e instale o modelo de formulário adaptável usando o Gerenciador de pacotes](assets/populate-with-xml.zip)
* [Baixe e instale o formulário adaptável de exemplo](assets/populate-af-with-query-paramters-form.zip)

* [Pré-visualizar o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Você deve ver o formulário adaptável preenchido com o valor John e Doe
