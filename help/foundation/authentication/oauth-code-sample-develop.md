---
title: Desenvolvimento de escopos OAuth no AEM
description: Os Escopos OAuth extensíveis da Adobe Experience Manager permitem o controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo de solicitação no contexto de AEM.
version: 6.3, 6.4, 6.5
feature: Usuários e grupos
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---


# Desenvolvimento de escopos OAuth

Os escopos extensíveis OAuth da Adobe Experience Manager permitem o controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo de solicitação no contexto de AEM.

![Fluxo de escopos Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fornece três escopos:

* Perfil
* Acesso offline
* Replicar

AEM escopos OAuth extensíveis permitem que outros escopos personalizados sejam definidos. Por exemplo, um escopo personalizado pode ser desenvolvido e implantado em AEM que permite que um aplicativo móvel autorizado por meio do OAuth seja restrito à leitura, mas não à gravação de ativos.

O OAuth é o método preferido para autorizar um aplicativo cliente, pois ele usa um token de acesso em vez de exigir que as credenciais de um usuário AEM sejam fornecidas a esse aplicativo.

* [Visualizar o código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
