---
title: Acionar o fluxo de trabalho de AEM no envio de formulário HTM5 - Revisar e aprovar PDF
description: Continue preenchendo o formulário para publicação de conteúdo para dispositivos móveis no modo offline e envie o formulário para publicação de conteúdo para dispositivos móveis para acionar o fluxo de trabalho do AEM
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
duration: 50
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

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
