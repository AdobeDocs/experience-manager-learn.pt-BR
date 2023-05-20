---
title: Acionar o fluxo de trabalho de AEM no envio de formulário HTM5 - Revisar e aprovar PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continue preenchendo o formulário para publicação de conteúdo para dispositivos móveis no modo offline e envie o formulário para publicação de conteúdo para dispositivos móveis para acionar o fluxo de trabalho do AEM
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---

# Fluxo de trabalho para revisar e aprovar o PDF enviado

A última e última etapa é criar um workflow de AEM que gerará um PDF estático ou não interativo para revisão e aprovação. O fluxo de trabalho é acionado por meio de um Iniciador AEM configurado no nó `/content/pdfsubmissions`.

A captura de tela a seguir mostra as etapas envolvidas no fluxo de trabalho.

![fluxo de trabalho](assets/workflow.PNG)

## Etapa de geração de fluxo de trabalho de PDF não interativo

O modelo XDP e os dados a serem mesclados com o modelo são especificados aqui. Os dados a serem mesclados são os dados enviados do PDF. Esses dados enviados são armazenados no nó `/content/pdfsubmissions`.

![fluxo de trabalho](assets/generate-pdf1.PNG)

O PDF gerado é atribuído à variável de fluxo de trabalho chamada `submittedPDF`.

![fluxo de trabalho](assets/generate-pdf2.PNG)

### Atribuir o PDF gerado para revisão e aprovação

O componente de fluxo de trabalho Atribuir tarefa é usado aqui para atribuir o PDF gerado para revisão e aprovação. A variável `submittedPDF` é usado na guia Forms e Documentos do componente de fluxo de trabalho Atribuir tarefa.

![fluxo de trabalho](assets/assign-task.PNG)
