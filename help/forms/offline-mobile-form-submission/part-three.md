---
title: Acionar fluxo de trabalho do AEM no envio de formulário HTM5
seo-title: Acione o fluxo de trabalho do AEM no envio do formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar o fluxo de trabalho do AEM
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar o fluxo de trabalho do AEM
feature: Formulários para publicação de conteúdo para dispositivos móveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# Fluxo de trabalho para revisar e aprovar o PDF enviado

A última e última etapa é criar um fluxo de trabalho do AEM que gerará um PDF estático ou não interativo para revisão e aprovação. O fluxo de trabalho será acionado por meio de um AEM Launcher configurado no nó `/content/pdfsubmissions`.

A captura de tela a seguir mostra as etapas envolvidas no fluxo de trabalho.

![fluxo de trabalho](assets/workflow.PNG)

## Etapa de fluxo de trabalho Gerar PDF não interativo

O modelo XDP e os dados a serem unidos ao modelo são especificados aqui. Os dados a serem unidos são os dados enviados do PDF. Esses dados enviados são armazenados no nó `/content/pdfsubmissions`.

![fluxo de trabalho](assets/generate-pdf1.PNG)

O PDF gerado é atribuído à variável de fluxo de trabalho chamada `submittedPDF`.

![fluxo de trabalho](assets/generate-pdf2.PNG)

### Atribua o pdf gerado para revisão e aprovação

Atribuir componente de fluxo de trabalho da tarefa é usado aqui para atribuir o PDF gerado para revisão e aprovação. A variável `submittedPDF` é usada na guia Formulários e Documentos do componente do fluxo de trabalho Atribuir tarefa .

![fluxo de trabalho](assets/assign-task.PNG)
