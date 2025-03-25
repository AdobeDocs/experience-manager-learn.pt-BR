---
title: Acionar o fluxo de trabalho do AEM no envio do formulário do HTML5 - Revisar e aprovar PDF
description: Fluxo de trabalho para revisar o PDF enviado
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 32
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# Fluxo de trabalho para revisar e aprovar o PDF enviado

A última e última etapa é criar um fluxo de trabalho do AEM que gerará uma PDF estática ou não interativa para revisão e aprovação. O fluxo de trabalho é disparado por meio de um Iniciador do AEM configurado no nó `/content/formsubmissions`.

A captura de tela a seguir mostra as etapas envolvidas no fluxo de trabalho.

![fluxo de trabalho](assets/workflow.PNG)

## Etapa de fluxo de trabalho Gerar PDF não interativa

O modelo XDP e os dados a serem mesclados com o modelo são especificados aqui. Os dados a serem mesclados são os dados enviados da PDF. Estes dados enviados estão armazenados no nó ```/content/formsubmissions```

![fluxo de trabalho](assets/generate-pdf1.PNG)

O PDF gerado é atribuído à variável de fluxo de trabalho chamada `submittedPDF`.

![fluxo de trabalho](assets/generate-pdf2.PNG)

### Atribuir o PDF gerado para revisão e aprovação

O componente de fluxo de trabalho Atribuir tarefa é usado aqui para atribuir o PDF gerado para revisão e aprovação. A variável `submittedPDF` é usada na guia Forms e Documentos do componente de fluxo de trabalho Atribuir tarefa.

![fluxo de trabalho](assets/assign-task.PNG)


## Próximas etapas

[Implantar os ativos no seu ambiente](./deploy-assets.md)