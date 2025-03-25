---
title: Entrega do documento de comunicação interativa - AEM Forms do canal da Web
description: Entrega de documento de canal da Web por link no email
feature: Interactive Communication
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 60
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Entrega por email do documento de canal da Web

Depois de definir e testar o documento de comunicação interativa do canal da Web, é necessário um mecanismo de entrega para entregar o documento do canal da Web ao destinatário.

Neste artigo, analisamos o email como um mecanismo de entrega para o documento de canal da Web. O recipient receberá um link para o documento de canal da Web por email.Ao clicar no link, o usuário é solicitado a autenticar e o documento de canal da Web é preenchido com os dados específicos do usuário conectado.

Vamos observar o seguinte fragmento de código. Esse código faz parte do GET.jsp que é acionado quando o usuário clica no link no email para exibir o documento do canal da Web. Nós obtemos o usuário logado usando o UserManager jackrabbit. Depois que obtemos o usuário conectado, obtemos o valor da propriedade accountNumber associada ao perfil do usuário.

Em seguida, associamos o valor accountNumber a uma chave chamada accountnumber no mapa. A chave **accountnumber** está definida no modal de dados de formulário como um Atributo de solicitação. O valor desse atributo é passado como parâmetro de entrada para o método do serviço de leitura do Modal de dados de formulário.

Linha 7: estamos enviando a solicitação recebida para outro servlet, com base no tipo de recurso identificado pelo URL do documento de comunicação interativa. A resposta retornada por esse segundo servlet é incluída na resposta do primeiro servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![Incluir abordagem de método](assets/includemethod.jpg)

Representação visual do código da linha 7

![Solicitar configuração de parâmetro](assets/requestparameter.png)

Atributo de solicitação definido para o serviço de leitura do modal de dados de formulário

[Pacote de Exemplo do AEM](assets/webchanneldelivery.zip).
