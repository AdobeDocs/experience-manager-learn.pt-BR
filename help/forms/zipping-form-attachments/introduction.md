---
title: Enviar anexos de formulário adaptáveis
description: Enviar anexos de formulário adaptáveis usando o componente Enviar email
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 2%

---

# Introdução



Caso de uso comum é enviar os anexos de formulário adaptáveis usando o componente Enviar email em um fluxo de trabalho de AEM.
Os clientes normalmente compactariam os anexos do formulário ou enviariam os anexos como arquivos individuais usando o componente Enviar email.

## Envie os anexos de formulário em um arquivo zip

Para realizar o caso de uso, uma etapa do processo de fluxo de trabalho personalizado foi gravada. Nesta etapa do processo personalizado, um arquivo zip com os anexos do formulário em criado e armazenado na pasta payload em um arquivo chamado *zipped_attachment.zip*

![anexos de formulário de envio](assets/send-form-attachments.JPG)

## Enviar os anexos de formulário individualmente

Para realizar esse caso de uso, uma etapa do processo de fluxo de trabalho personalizado foi gravada. Nesta etapa do processo personalizado, preenchemos variáveis de fluxo de trabalho do tipo ArrayList of Documents e ArrayList of Strings.

![lista de envio de documentos](assets/send-list-of-documents.JPG)

## Próximas etapas

[Anexos de formulário zip](./custom-process-step.md)
