---
title: Personalizar componente de resumo
description: Estenda o componente de etapa de resumo para incluir a capacidade de navegar até o próximo formulário no pacote.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Personalizar etapa de resumo

O componente Etapa de resumo é usado para exibir o resumo do envio do formulário com um link para baixar o formulário assinado. A etapa de resumo geralmente é colocada no último painel do formulário.
Para o propósito deste caso de uso, criamos um novo componente baseado no componente Resumo pronto para uso e estendemos a capacidade de incluir clientlib personalizado.

Esse componente é identificado pelo rótulo Assinar vários formulários

A captura de tela a seguir mostra o novo componente criado para exibir a mensagem ao concluir a cerimônia de assinatura

![componente de resumo](assets/summary.PNG)

O novo componente é baseado no componente de resumo pronto para uso.
![component-prop](assets/componentprop.PNG)

Adicionamos um botão para navegar até o próximo formulário para assinatura
![código do modelo](assets/template-code.PNG)

O summary.jsp tem o seguinte código. Ela tem referência à biblioteca do cliente identificada pela id da categoria **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

O componente de resumo personalizado pode ser [baixado aqui](assets/custom-summary-step.zip)

## Próximas etapas

[Obter o próximo formulário para assinatura](./create-client-lib.md)