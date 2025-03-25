---
title: Acionar o fluxo de trabalho do AEM no envio do formulário do PDF
description: Continue preenchendo o formulário para publicação de conteúdo para dispositivos móveis no modo offline e envie o formulário para publicação de conteúdo para dispositivos móveis para acionar o fluxo de trabalho do AEM
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Download do formulário móvel parcialmente concluído e envio para acionar um fluxo de trabalho do AEM

Um caso de uso comum é a capacidade de renderizar o XDP como HTML para atividades de captura de dados. Isso funciona bem quando os formulários são simples e podem ser preenchidos e enviados online. No entanto, se o formulário for complexo, os usuários talvez não consigam preenchê-lo online, precisamos fornecer a capacidade de permitir que os preenchimentos de formulário baixem a versão interativa do formulário a ser preenchido usando o Acrobat/Reader offline. Depois que o formulário for preenchido, o usuário poderá estar online para enviar o formulário.
Para realizar esse caso de uso, precisamos executar as seguintes etapas:

* Capacidade de gerar PDF interativo/preenchível com os dados inseridos no formulário móvel
* Lidar com o envio do PDF pelo Acrobat/Reader
* Acione o fluxo de trabalho do Adobe Experience Manager (AEM) para revisar o PDF enviado

Este tutorial aborda as etapas necessárias para realizar o caso de uso acima. Exemplos de código e ativos relacionados a este tutorial estão [disponíveis aqui.](./deploy-assets.md)

O vídeo a seguir fornece uma visão geral do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## Próximas etapas

[Criar perfil personalizado](./custom-profile.md)