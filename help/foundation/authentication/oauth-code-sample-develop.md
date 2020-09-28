---
title: Desenvolvimento de escopos OAuth em AEM
description: Os Escopos OAuth extensíveis da Adobe Experience Manager permitem controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo de solicitação no contexto de AEM.
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: develop
audience: developer
doc-type: code
translation-type: tm+mt
source-git-commit: b351a57e6e5be0fe5696dc09842fa77fdd036a27
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---


# Desenvolvendo escopos OAuth

Os escopos OAuth extensíveis da Adobe Experience Manager permitem controle de acesso para recursos de um aplicativo cliente autorizado por um usuário final. O diagrama abaixo ilustra o fluxo de solicitação no contexto de AEM.

![Fluxo de Escopos de Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fornece três escopos:

* Perfil
* Acesso offline
* Replicar

AEM escopos OAuth extensíveis permitem que outros escopos personalizados sejam definidos. Por exemplo, um escopo personalizado pode ser desenvolvido e implantado em AEM que permite que um aplicativo móvel autorizado via OAuth seja restrito à leitura, mas não à gravação de ativos.

O OAuth é o método preferido para autorizar um aplicativo cliente, pois ele usa um token de acesso em vez de exigir que as credenciais de um usuário AEM sejam fornecidas a esse aplicativo.

* [Visualização do código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
