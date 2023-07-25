---
title: Relatório sobre campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS com o Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
kt: 12557
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# Criar elementos de dados

Na propriedade Tags, adicionamos dois novos elementos de dados (ApplicantsStateOfResidence e validationError).

![formulário adaptável](assets/data_elements.png)

## EstadoDeResidênciaCandidato

A variável **EstadoDeResidênciaCandidato** o elemento de dados foi configurado ao selecionar **Núcleo** na lista suspensa extensão e **Custom Code** para o Tipo de elemento de dados, como mostrado na captura de tela abaixo
![requerente-Estado-residência](assets/applicantstateofresidence.png)

O código personalizado a seguir foi usado para capturar o valor do **_state_** campo de formulário adaptável.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

A variável **ValidationError** o elemento de dados foi configurado ao selecionar **Núcleo** na lista suspensa extensão e **Custom Code** para o Tipo de elemento de dados, como mostrado na captura de tela abaixo

![validation-error](assets/validation-error.png)

O código personalizado a seguir foi gravado para definir a `validationError` valor do elemento de dados.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## Próximas etapas

[Criar regras](./rules.md)