---
title: Uso de funções e do editor de código
description: Uso de funções e do editor de código para criar uma regra de negócios
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 466
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Uso de funções personalizadas e do editor de código {#using-functions-and-code-editor}

Nesta parte, usaremos funções personalizadas e o editor de código para criar regras de negócios.

você já instalou o [ClientLib com função personalizada](assets/client-libs-and-logo.zip) anteriormente neste tutorial.

Normalmente, uma biblioteca do cliente consiste em arquivos CSS e Javascript. Essa biblioteca do cliente contém um arquivo javascript que expõe uma função para preencher valores de listas suspensas.


## Função para preencher a lista suspensa {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Definir título de resumo do painel {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Painel Validar {#validate-panels-using-rule-editor}

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

Linha 9 - Se nenhum erro for movido para o próximo painel

Pré-visualize o formulário e teste a funcionalidade recém-ativada.
