---
title: Enviar anexos de formulário adaptáveis
description: Enviar anexos de formulário adaptáveis usando o componente Enviar email
feature: Formulários adaptáveis
version: 6.5
topic: Desenvolvimento
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

---


# Introdução



Caso de uso comum é enviar os anexos de formulário adaptáveis usando o componente Enviar email em um fluxo de trabalho de AEM.
Os clientes normalmente compactariam os anexos do formulário ou enviariam os anexos como arquivos individuais usando o componente Enviar email.

## Envie os anexos de formulário em um arquivo zip

Para realizar o caso de uso, uma etapa do processo de fluxo de trabalho personalizado foi gravada. Nesta etapa do processo personalizado, um arquivo zip com os anexos de formulário criados e armazenados na pasta carga em um arquivo chamado *zipped_attachments.zip*

![anexos de formulário de envio](assets/send-form-attachments.JPG)

## Enviar os anexos de formulário individualmente

Para realizar esse caso de uso, uma etapa do processo de fluxo de trabalho personalizado foi gravada. Nesta etapa do processo personalizado, preenchemos variáveis de fluxo de trabalho do tipo ArrayList of Documents e ArrayList of Strings.

![lista de envio de documentos](assets/send-list-of-documents.JPG)



