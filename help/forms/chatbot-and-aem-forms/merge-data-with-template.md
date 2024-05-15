---
title: Mesclar dados com o modelo XDP
description: Criar documento PDF mesclando dados com o modelo
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Mesclar dados com o modelo XDP

A próxima etapa é mesclar os dados XML com o modelo para gerar o PDF. Esse PDF é enviado para assinaturas usando o Adobe Sign.

## Uso do OutputService para gerar o PDF

A variável [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) o método do OutputService foi usado para gerar o PDF.
O PDF gerado foi enviado para assinatura usando a API REST do Adobe Sign.
