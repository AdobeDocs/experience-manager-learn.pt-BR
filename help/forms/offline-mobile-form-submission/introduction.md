---
title: Acionar fluxo de trabalho AEM na introdução do Envio de formulário HTML5
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Download de formulário móvel parcialmente preenchido e envio para AEM fluxo de trabalho

Um caso de uso comum é ter a capacidade de renderizar o XDP como HTML para atividades de captura de dados. Isso funciona bem quando os formulários são simples e podem ser preenchidos e enviados online. No entanto, se o formulário for complexo, os usuários talvez não consigam preenchê-lo on-line, precisamos fornecer a capacidade de permitir que os usuários baixem a versão interativa do formulário a ser preenchido usando o Acrobat/Reader de maneira offline. Depois que o formulário for preenchido, o usuário poderá ficar online para enviar o formulário.
Para realizar esse caso de uso, precisamos executar as seguintes etapas:

* Capacidade de gerar PDF interativo/preenchível com os dados inseridos no formulário móvel
* Gerenciar o envio do PDF do Acrobat/Reader
* Acione o fluxo de trabalho do Adobe Experience Manager (AEM) para revisar o PDF enviado

Este tutorial percorre as etapas necessárias para realizar o caso de uso acima. O código de amostra e os ativos relacionados a este tutorial são [disponível aqui.](part-four.md)

O vídeo a seguir fornece uma visão geral do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
