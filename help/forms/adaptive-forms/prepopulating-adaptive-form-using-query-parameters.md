---
title: Preencha o Adaptive Forms usando parâmetros de consulta.
description: Preencha o Adaptive Forms com dados dos parâmetros de consulta.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Preencher previamente o Adaptive Forms usando parâmetros de consulta

Um de nossos clientes tinha o requisito de preencher formulários adaptáveis usando os parâmetros de consulta. Por exemplo, no URL a seguir, os campos FirstName e LastName no formulário adaptável são definidos como John e Doe, respectivamente

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Para realizar esse caso de uso, um novo modelo de formulário adaptável foi criado e associado a um componente de página. Neste componente de página, temos um jsp para obter os parâmetros de consulta e criar uma estrutura xml que pode ser usada para preencher o formulário adaptável.

Os detalhes sobre a criação de um novo modelo de formulário adaptável e componente da página são [explicado neste vídeo.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

O código a seguir é o usado na página jsp

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
>Se o formulário estiver usando um esquema, a estrutura do xml será diferente e será necessário criar o xml de acordo.


## Implante os ativos em seu sistema

* [Baixe e instale o modelo de formulário adaptável usando o Gerenciador de pacotes](assets/populate-with-xml.zip)
* [Baixe e instale o formulário adaptável de amostra](assets/populate-af-with-query-paramters-form.zip)

* [Visualizar o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Você deve ver o formulário adaptável preenchido com o valor John e Doe
