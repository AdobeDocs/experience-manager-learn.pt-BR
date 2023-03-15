---
title: Relatar campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS ao Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Teste sua solução

Visualize e envie seu formulário usando várias combinações de valores de formulário. Permita de vários a 30 minutos para visualizar seus dados nos relatórios do Adobe Analytics. Os dados definidos para props são exibidos no relatório antes dos dados definidos para eVars.

## Conjunto de relatórios

Os dados do formulário capturados no Adobe Analytics são apresentados em formato de rosca

**Submissões por Estado**

![applicantsbystate](assets/donut.png)

Erros de validação de campo

![field-validation-error](assets/donut-field-validation.png)

## Depuração

Verifique se o Formulário adaptável está usando o mesmo contêiner de configuração que contém a Configuração do Adobe Launch.

Para confirmar que o formulário está enviando dados para o Adobe Analytics, faça o seguinte

* Abra as Ferramentas do desenvolvedor em seu navegador.
* Digite o seguinte texto no painel Console.

```javascript
_satellite.setDebug(true)
```

Interaja com o formulário enquanto mantém a janela do console aberta. Você deveria ver algo assim

![console-debug](assets/debug.png)

## Usar o Adobe Experience Platform Debugger

Adicione o [Extensão do AEP Debugger](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) no navegador (é necessário fazer logon) para obter mais informações de depuração

![platform-debugger](assets/platform-debugger.png)





