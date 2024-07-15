---
title: Relatório sobre campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS com o Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# Testar sua solução

Pré-visualize e envie seu formulário usando várias combinações de valores de formulário. Aguarde de vários a 30 minutos para ver seus dados nos relatórios do Adobe Analytics. O conjunto de dados para props é exibido nos relatórios antes dos dados definidos para eVars.

## Conjunto de relatórios

Os dados de formulário capturados no Adobe Analytics são apresentados no formato de rosca

**Envios por Estado**

![applicantsbystate](assets/donut.png)

Erros de validação de campo

![erro-validação-campo](assets/donut-field-validation.png)

## Depuração

Verifique se o formulário adaptável está usando o mesmo contêiner de configuração que contém a configuração do Adobe Launch.

Para confirmar se o formulário está enviando dados para o Adobe Analytics, faça o seguinte

* Abra as Ferramentas do desenvolvedor em seu navegador.
* Insira o seguinte texto no painel Console.

```javascript
_satellite.setDebug(true)
```

Interaja com o formulário enquanto mantém a janela do console aberta. Você deve ver algo como isso

![depuração-console](assets/debug.png)

## Usar Adobe Experience Platform Debugger

Adicione a [extensão do depurador da AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) ao seu navegador (você precisa entrar) para obter mais informações sobre depuração

![depurador-plataforma](assets/platform-debugger.png)

## Parabéns

Você integrou com êxito o AEM Forms as a Cloud Service ao Adobe Analytics para criar relatórios sobre campos de dados de formulário.