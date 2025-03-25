---
title: Personalizar o componente de Resumo
description: Estender o componente da etapa de resumo para incluir a capacidade de navegar até o próximo formulário no pacote.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
duration: 38
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Personalizar etapa de resumo

O componente da etapa de resumo é usado para exibir o resumo do envio do formulário com um link para baixar o formulário assinado. A etapa de resumo normalmente é colocada no último painel do formulário.
Para o propósito deste caso de uso, criamos um novo componente com base no componente Resumo pronto para uso e estendemos a capacidade para incluir clientlib personalizado.

Este componente é identificado pelo rótulo Assinar formulário múltiplo

A captura de tela a seguir mostra o novo componente criado para exibir a mensagem na conclusão da cerimônia de assinatura

![componente de resumo](assets/summary.PNG)

O novo componente é baseado no componente de resumo pronto para uso.
![propriedade-componente](assets/componentprop.PNG)

Adicionamos um botão para navegar até o próximo formulário para assinatura
![código-modelo](assets/template-code.PNG)

O summary.jsp tem o seguinte código. Ela tem referência à biblioteca do cliente identificada pela ID de categoria **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Ativos

O componente de resumo personalizado pode ser [baixado daqui](assets/custom-summary-step.zip)

## Próximas etapas

[Obter o próximo formulário para assinatura](./create-client-lib.md)