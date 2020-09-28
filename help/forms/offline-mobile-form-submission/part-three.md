---
title: Acionar fluxo de trabalho AEM no envio de formulário HTM5
seo-title: Acionar fluxo de trabalho AEM no envio de formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c56942831614b981684861ea78f1bd15f3bb1ab9
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 2%

---


# Fluxo de trabalho para revisar e aprovar o PDF enviado

A última e última etapa é criar AEM fluxo de trabalho que gerará um PDF estático ou não interativo para revisão e aprovação. O fluxo de trabalho será acionado por meio de um Iniciador AEM configurado no nó `/content/pdfsubmissions`.

A captura de tela a seguir mostra as etapas envolvidas no fluxo de trabalho.

![fluxo de trabalho](assets/workflow.PNG)

## Gerar etapa de fluxo de trabalho de PDF não interativo

O modelo XDP e os dados a serem unidos ao modelo são especificados aqui. Os dados a serem unidos são os dados enviados do PDF. Esses dados enviados são armazenados sob o nó `/content/pdfsubmissions`.

![fluxo de trabalho](assets/generate-pdf1.PNG)

O PDF gerado é atribuído à variável de fluxo de trabalho chamada `submittedPDF`.

![fluxo de trabalho](assets/generate-pdf2.PNG)

### Atribua o pdf gerado para revisão e aprovação

Atribuir componente de fluxo de trabalho de tarefa é usado aqui para atribuir o PDF gerado para revisão e aprovação. A variável `submittedPDF` é usada na guia Forms e Documentos do componente de fluxo de trabalho Atribuir Tarefa.

![fluxo de trabalho](assets/assign-task.PNG)
