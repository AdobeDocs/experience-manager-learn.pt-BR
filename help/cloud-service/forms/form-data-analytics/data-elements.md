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
source-wordcount: '129'
ht-degree: 0%

---

# Criar elementos de dados apropriados

Na propriedade Tags , adicionamos dois novos elementos de dados (RequerentesStateOfResidence e validationError).

![forma adaptável](assets/data_elements.png)

## RequerenteEstadoDeResidência

O **RequerenteEstadoDeResidência** o elemento de dados foi configurado ao selecionar **Núcleo** no menu suspenso de extensão e **Código personalizado** para o Tipo de elemento de dados, como mostrado na captura de tela abaixo
![requerente-Estado-residência](assets/applicantstateofresidence.png)

O código personalizado a seguir foi usado para capturar o valor da variável **_state_** campo de formulário adaptável.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

O **ValidaçãoErro** o elemento de dados foi configurado ao selecionar **Núcleo** no menu suspenso de extensão e **Código personalizado** para o Tipo de elemento de dados, como mostrado na captura de tela abaixo

![erro de validação](assets/validation-error.png)

O código personalizado a seguir foi gravado para definir o valor do elemento de dados validationError.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```