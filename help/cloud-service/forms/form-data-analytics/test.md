---
title: Relatório sobre campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS com o Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 4100061624bd8955bee392f1eced20f388f2902c
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Testar sua solução

Pré-visualize e envie seu formulário usando várias combinações de valores de formulário. Aguarde de vários a 30 minutos para ver seus dados nos relatórios do Adobe Analytics. O conjunto de dados para props é exibido nos relatórios antes dos dados definidos para eVars.

## Conjunto de relatórios

Os dados de formulário capturados no Adobe Analytics são apresentados em formato de rosca. Envios por estado

![applicantsbystate](assets/donut.png)

Erros de validação de campo

![erro de validação de campo](assets/donut-field-validation.png)

## Depuração

Verifique se o formulário adaptável está usando o mesmo contêiner de configuração que contém a configuração do Adobe Launch.

Para confirmar se o formulário está enviando dados para o Adobe Analytics, faça o seguinte

* Abra as Ferramentas do desenvolvedor em seu navegador.
* Insira o seguinte texto no painel Console.

```javascript
_satellite.setDebug(true)
```

Interaja com o formulário enquanto mantém a janela do console aberta. Você deve ver algo como isso

![console-debug](assets/debug.png)

## Usar o Adobe Experience Platform Debugger

Adicione o [Extensão do depurador da AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) no navegador (é necessário fazer logon) para obter mais informações de depuração

![platform-debugger](assets/platform-debugger.png)





