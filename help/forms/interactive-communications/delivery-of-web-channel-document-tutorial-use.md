---
title: Entrega do Documento de comunicação interativa - Formulários AEM de canal da Web
seo-title: Entrega do Documento de comunicação interativa - Formulários AEM de canal da Web
description: Entrega do documento do canal da Web via link no email
seo-description: Entrega do documento do canal da Web via link no email
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---


# Entrega de email do documento de canal da Web

Depois de definir e testar seu documento de comunicação interativa de canal da Web, você precisa de um mecanismo de delivery para entregar o documento de canal da Web ao recipient.

Neste artigo, analisamos o email como um mecanismo de delivery para documentos de canal da Web. O recipient obterá um link para o documento do canal da Web via email.Ao clicar no link, o usuário será solicitado a autenticar e o documento do canal da Web será preenchido com os dados específicos do usuário conectado.

Vamos observar o seguinte trecho de código. Este código faz parte de GET.jsp, que é acionado quando o usuário clica no link no email para exibir o documento do canal da Web. Obtemos o usuário conectado usando o jackrabbit UserManager. Depois que obtemos o usuário conectado, obtemos o valor da propriedade accountNumber associada ao perfil do usuário.

Em seguida, associamos o valor accountNumber a uma chave chamada número de conta no mapa. A chave **accountnumber** é definida no modal de dados de formulário como um Atributo de solicitação. O valor desse atributo é passado como um parâmetro de entrada para o método de serviço de leitura Form Data Modal.

Linha 7: Estamos enviando a solicitação recebida para outro servlet, com base no tipo de recurso identificado pelo url do Documento de Comunicação Interativa. A resposta retornada por este segundo servlet é incluída na resposta do primeiro servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![includemethod](assets/includemethod.jpg)

Representação visual do código da linha 7

![requestparameter](assets/requestparameter.png)

Atributo de solicitação definido para o serviço de leitura do modal de dados do formulário


[Pacote AEM de exemplo](assets/webchanneldelivery.zip).
