---
title: Enviar dados para a lista do SharePoint usando a etapa de fluxo de trabalho
description: Inserir dados na lista do SharePoint usando a etapa de fluxo de trabalho invocar FDM
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
exl-id: b369ed05-ba25-4b0e-aa3b-e7fc1621067d
duration: 52
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 1%

---

# Inserir dados na lista do SharePoint usando a etapa de fluxo de trabalho Chamar FDM


Este artigo explica as etapas necessárias para inserir dados na lista do SharePoint usando a etapa invocar FDM no fluxo de trabalho AEM.

Este artigo supõe que você tenha [configurado com êxito um formulário adaptável para enviar dados à lista do SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## Criar um modelo de dados de formulário com base na fonte de dados da lista do SharePoint

* Crie um novo modelo de dados de formulário com base na fonte de dados da lista do SharePoint.
* Adicione o modelo apropriado e o serviço get do modelo de dados de formulário.
* Configure o serviço insert para inserir o objeto de modelo de nível superior.
* Teste o serviço de inserção.


## Criar um fluxo de trabalho

* Crie um fluxo de trabalho simples com a etapa invocar FDM.
* Configure a etapa invocar FDM para usar o modelo de dados de formulário criado na etapa anterior.
* ![associate-fdm](assets/fdm-insert-1.png)

## Formulário adaptável baseado em componentes principais

Os dados enviados estão no formato a seguir. Precisamos extrair o objeto ContactUS usando a notação de pontos na etapa de fluxo de trabalho invocar serviço de modelo de dados de formulário, conforme mostrado na captura de tela

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```


* ![mapear-parâmetros-entrada](assets/fdm-insert-2.png)


## Formulário adaptável baseado em componentes de base

Os dados enviados estão no formato a seguir. Extraia o objeto JSON ContactUS usando a notação de pontos na etapa de fluxo de trabalho Chamar serviço de modelo de dados de formulário

```json
{
    "afData": {
        "afUnboundData": {
            "data": {}
        },
        "afBoundData": {
            "data": {
                "ContactUS": {
                    "Title": "Lord",
                    "HighNetWorth": "true",
                    "SubmitterName": "John Doe",
                    "Products": "Forms"
                }
            }
        },
        "afSubmissionInfo": {
            "lastFocusItem": "guide[0].guide1[0].guideRootPanel[0].afJsonSchemaRoot[0]",
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/foundationform",
            "afSubmissionTime": "20240517100126"
        }
    }
}
```

![formulário baseado em fundação](assets/foundation-based-form.png)

## Configurar o formulário adaptável para acionar o fluxo de trabalho do AEM

* Crie o formulário adaptável usando o Modelo de dados de formulário criado na etapa anterior.
* Arraste e solte alguns campos da fonte de dados no formulário.
* Configure a ação de envio do formulário conforme mostrado abaixo
* ![ação-envio](assets/configure-af.png)



## Testar o formulário

Visualize o formulário criado na etapa anterior. Preencha o formulário e envie. Os dados do formulário devem ser inseridos na lista do SharePoint.
