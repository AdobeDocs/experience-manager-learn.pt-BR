---
title: Enviar anexos de formulário adaptáveis
description: Enviar anexos de formulário adaptáveis usando o componente de envio de email
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



Um caso de uso comum é o envio de anexos de formulário adaptáveis usando o componente de envio de email em um fluxo de trabalho AEM.
Em geral, os clientes compactam os anexos do formulário ou os enviam como arquivos individuais usando o componente de envio de email.

## Enviar os anexos de formulário em um arquivo zip

Para realizar o caso de uso, uma etapa de processo de fluxo de trabalho personalizada foi escrita. Nesta etapa do processo personalizado, um arquivo zip com os anexos de formulário em é criado e armazenado na pasta de carga em um arquivo chamado *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## Enviar os anexos do formulário individualmente

Para realizar esse caso de uso, uma etapa de processo de fluxo de trabalho personalizada foi escrita. Nesta etapa de processo personalizada, preenchemos variáveis de fluxo de trabalho do tipo ArrayList of Documents e ArrayList of Strings.

![send-list-of-documents](assets/send-list-of-documents.JPG)

## Próximas etapas

[Anexos de formulário Zip](./custom-process-step.md)
