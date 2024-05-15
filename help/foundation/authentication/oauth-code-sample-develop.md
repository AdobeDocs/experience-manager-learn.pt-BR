---
title: Desenvolvimento de escopos OAuth em AEM
description: Os escopos OAuth extensíveis do Adobe Experience Manager permitem o controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo da solicitação no contexto do AEM.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# Desenvolvimento de escopos do OAuth

Os escopos OAuth extensíveis do Adobe Experience Manager permitem o controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo da solicitação no contexto do AEM.

![Fluxo De Escopos Do Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

O AEM fornece três escopos:

* Perfil
* Acesso off-line
* Replicar

Escopos OAuth extensíveis para AEM permitem que outros escopos personalizados sejam definidos. Por exemplo, um escopo personalizado pode ser desenvolvido e implantado no AEM, permitindo que um aplicativo móvel autorizado por meio do OAuth seja restrito à leitura, mas não à gravação de ativos.

OAuth é o método preferido para autorizar um aplicativo cliente, pois usa um token de acesso em vez de exigir que as credenciais de um usuário do AEM sejam fornecidas a esse aplicativo.

* [Exibir o código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
