---
title: Criar uma configuração de serviços em nuvem
description: Criar fonte de dados para se conectar ao Salesforce usando as credenciais do OAuth
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 16%

---

# Criar Source de dados

Crie uma fonte de dados com suporte de REST usando o arquivo swagger criado na etapa anterior.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| Configuração | Valor |
|---------------------|-----------------------------------------------------------------|
| URL do OAuth | https://login.salesforce.com/services/oauth2/authorize |
| Escopo da autorização | api chatter_api id completa openid refresh_token visualforce web |
| URL do token de atualização | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| URL do token de acesso | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Os nomes de domínio da URL do token de atualização e acesso terão que ser alterados para corresponder às configurações da sua conta da Salesforce**