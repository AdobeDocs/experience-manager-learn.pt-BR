---
title: Delivery de Interative Communication Document - Web Channel AEM Forms
description: Entrega do documento do canal da Web via link no email
feature: Interactive Communication
audience: developer
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# Entrega de email do documento de canal da Web

Depois de definir e testar seu documento de comunicação interativa de canal da Web, você precisa de um mecanismo de delivery para entregar o documento de canal da Web ao recipient.

Neste artigo, analisamos o email como um mecanismo de delivery para documentos de canal da Web. O recipient obterá um link para o documento do canal da Web via email.Ao clicar no link, o usuário é solicitado a autenticar e o documento do canal da Web é preenchido com os dados específicos do usuário conectado.

Vamos observar o seguinte trecho de código. Esse código faz parte do GET.jsp, que é acionado quando o usuário clica no link no email para exibir o documento do canal da Web. Obtemos o usuário conectado usando o jackrabbit UserManager. Depois que obtemos o usuário conectado, obtemos o valor da propriedade accountNumber associada ao perfil do usuário.

Em seguida, associamos o valor accountNumber a uma chave chamada número de conta no mapa. A chave **accountnumber** é definida no modal de dados do formulário como um Atributo de solicitação. O valor desse atributo é passado como um parâmetro de entrada para o método de serviço de leitura Form Data Modal.

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

![Incluir método](assets/includemethod.jpg)

Representação visual do código da linha 7

![Configurar parâmetro de solicitação](assets/requestparameter.png)

Atributo de solicitação definido para o serviço de leitura do modal de dados do formulário

[Pacote de AEM de exemplo](assets/webchanneldelivery.zip).
