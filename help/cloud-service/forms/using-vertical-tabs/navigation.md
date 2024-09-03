---
title: Utilização de guias verticais no Cloud Service do AEM Forms
description: Criar um formulário adaptável usando guias verticais
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Navegar entre as guias

Você pode navegar entre a guia clicando nas guias individuais ou usando os botões anterior e próximo no formulário.
Para navegar usando botões, adicione dois botões ao formulário e nomeie-os como Anterior e Próximo. Associe a seguinte função personalizada ao evento de clique do botão para navegar entre as guias.

Veja a seguir a função personalizada para navegar entre as guias.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

Veja a seguir o editor de regras para os botões Próximo e Anterior

**Próximo Botão**

![botão Avançar](assets/next-button.png)

**Botão Anterior**

![botão-anterior](assets/prev-button.png)
