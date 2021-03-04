---
title: Desenvolvimento de escopos OAuth no AEM
description: Os extensíveis escopos OAuth do Adobe Experience Manager permitem o controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo de solicitação no contexto do AEM.
version: 6.3, 6.4, 6.5
feature: 'Usuários e grupos '
topics: authentication, security
activity: develop
audience: developer
doc-type: code
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 3%

---


# Desenvolvimento de escopos OAuth

Os escopos OAuth extensíveis do Adobe Experience Manager permitem o controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo de solicitação no contexto do AEM.

![Fluxo de escopos Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

O AEM fornece três escopos:

* Perfil
* Acesso offline
* Replicar

Os escopos OAuth extensíveis do AEM permitem que outros escopos personalizados sejam definidos. Por exemplo, um escopo personalizado pode ser desenvolvido e implantado no AEM, permitindo que um aplicativo móvel autorizado por meio do OAuth seja restrito à leitura, mas não à gravação de ativos.

O OAuth é o método preferido para autorizar um aplicativo cliente, pois ele usa um token de acesso em vez de exigir que as credenciais de um usuário do AEM sejam fornecidas a esse aplicativo.

* [Visualizar o código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
