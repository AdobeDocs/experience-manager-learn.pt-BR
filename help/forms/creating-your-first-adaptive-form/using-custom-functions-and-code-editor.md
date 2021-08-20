---
title: Uso de funções e editor de código
description: Uso do editor de funções e códigos para criar regras de negócios
feature: Formulários adaptáveis
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 2%

---


# Uso de funções personalizadas e do editor de códigos {#using-functions-and-code-editor}

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
