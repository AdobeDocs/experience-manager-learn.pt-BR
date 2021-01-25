---
title: Personalizar componente de resumo
description: Estende o componente de etapa de resumo para incluir a capacidade de navegar até o próximo formulário no pacote.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Personalizar etapa de resumo

O componente da etapa de resumo é usado para exibir o resumo do envio do formulário com um link para baixar o formulário assinado. A etapa de resumo normalmente é colocada no último painel do formulário.
Para a finalidade desse caso de uso, criamos um novo componente com base no componente Resumo pronto para uso e estendemos a capacidade de incluir clientlib personalizado.

Este componente é identificado pelo rótulo Assinar vários formulários

A seguinte captura de tela mostra o novo componente criado para exibir a mensagem após a conclusão da cerimônia de assinatura

![componente de resumo](assets/summary.PNG)

O novo componente é baseado no componente de resumo da caixa.
![componente-prop](assets/componentprop.PNG)

Adicionamos um botão para navegar até o próximo formulário para assinatura
![código-modelo](assets/template-code.PNG)

O summary.jsp tem o seguinte código. Ela tem referência à biblioteca do cliente identificada pela ID da categoria **getnextform**

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


