---
title: Delivery de Documento de comunicação interativa - AEM Forms Canal da Web
seo-title: Delivery de Documento de comunicação interativa - AEM Forms Canal da Web
description: Delivery do documento do canal da Web via link no e-mail
seo-description: Delivery do documento do canal da Web via link no e-mail
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Delivery de e-mail do Documento de Canal da Web

Depois de definir e testar o documento de comunicação interativa do canal da Web, você precisará de um mecanismo de delivery para fornecer o documento do canal da Web ao recipient.

Neste artigo, olhamos para o email como um mecanismo de delivery para o documento de canais da Web. O recipient receberá um link para o documento do canal da Web por email.Ao clicar no link, o usuário será solicitado a autenticar e o documento do canal da Web será preenchido com os dados específicos do usuário conectado.

Vamos ver o seguinte trecho de código. Esse código faz parte do GET.jsp, que é acionado quando o usuário clica no link no email para visualização do documento do canal da Web. O usuário conectado usa o Jackrabbit UserManager. Quando obtemos o usuário conectado, obtemos o valor da propriedade accountNumber associada ao perfil do usuário.

Em seguida, associamos o valor accountNumber a uma chave chamada account number no mapa. A tecla **accountnumber** está definida no formato modal de dados como um Atributo de solicitação. O valor desse atributo é passado como um parâmetro de entrada para o método de serviço de leitura do Form Data Modal.

Linha 7: Estamos enviando a solicitação recebida para outro servlet, com base no tipo de recurso identificado pelo url do Documento do Interative Communication. A resposta retornada por este segundo servlet está incluída na resposta do primeiro servlet.

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


[Amostra AEM pacote](assets/webchanneldelivery.zip).
