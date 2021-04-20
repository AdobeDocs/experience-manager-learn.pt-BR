---
title: Uso de funções e editor de código
seo-title: Uso de funções e editor de código
description: Uso do editor de funções e códigos para criar regras de negócios
seo-description: Uso do editor de funções e códigos para criar regras de negócios
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: Adaptive Forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 2%

---


# Uso de funções personalizadas e do editor de código {#using-functions-and-code-editor}

Nessa parte, usaremos funções personalizadas e o editor de códigos para criar regras de negócios.

você já instalou o [ClientLib com a função personalizada](assets/client-libs-and-logo.zip) anteriormente neste tutorial.

Normalmente, uma biblioteca do cliente consiste em arquivos CSS e Javascript. Essa biblioteca do cliente contém um arquivo javascript que expõe uma função para preencher valores de lista suspensa.


## Função para preencher a lista suspensa {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Definir título de resumo do painel {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Validar painel {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

Este é o código usado para validar campos do painel

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

Você pode remover o comentário da linha 1 para depurar o código na janela do navegador.

Linha 4 - Obter o painel atual

Linha 5 - Valide o painel atual.

Linha 9 - Se nenhum erro se mover para o painel seguinte

Visualize o formulário e teste a funcionalidade recém-ativada.
