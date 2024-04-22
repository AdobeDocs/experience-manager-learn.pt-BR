---
title: Mesclar dados com o modelo XDP
description: Criar documento PDF mesclando dados com o modelo
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Mesclar dados com o modelo XDP

A próxima etapa é mesclar os dados XML com o modelo para gerar o PDF. Esse PDF é enviado para assinaturas usando o Adobe Sign.

## Uso do OutputService para gerar o PDF

A variável [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) o método do OutputService foi usado para gerar o PDF.
O PDF gerado foi enviado para assinatura usando a API REST do Adobe Sign.

