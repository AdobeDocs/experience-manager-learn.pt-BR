---
title: Uso de funções e editor de código
description: Uso do editor de funções e códigos para criar regras de negócios
feature: Adaptive Forms
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Uso de funções personalizadas e do editor de códigos {#using-functions-and-code-editor}

Nessa parte, usaremos funções personalizadas e o editor de códigos para criar regras de negócios.

você já instalou o [ClientLib com função personalizada](assets/client-libs-and-logo.zip) anteriormente neste tutorial.

Normalmente, uma biblioteca do cliente consiste em arquivos CSS e Javascript. Essa biblioteca do cliente contém um arquivo javascript que expõe uma função para preencher valores de lista suspensa.


## Função para preencher a lista suspensa {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Definir título de resumo do painel {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Validar painel {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

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
