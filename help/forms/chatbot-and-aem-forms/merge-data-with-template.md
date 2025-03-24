---
title: Mesclar dados com o modelo XDP
description: Criar documento do PDF mesclando dados com o modelo
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 0%

---

# Mesclar dados com o modelo XDP

A próxima etapa é mesclar os dados XML com o modelo para gerar o PDF. Essa PDF é enviada para assinatura usando o Adobe Sign.

## Utilização do OutputService para gerar o PDF

O método [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) do OutputService foi usado para gerar o PDF.
O PDF gerado foi então enviado para assinaturas usando a API REST do Adobe Sign.
