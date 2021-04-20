---
title: Acionar fluxo de trabalho do AEM no envio de formulário HTM5
seo-title: Acione o fluxo de trabalho do AEM no envio do formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar o fluxo de trabalho do AEM
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar o fluxo de trabalho do AEM
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Download de formulário móvel parcialmente preenchido e envio para o fluxo de trabalho do AEM

Um caso de uso comum é ter a capacidade de renderizar o XDP como HTML para atividades de captura de dados. Isso funciona bem quando os formulários são simples e podem ser preenchidos e enviados online. No entanto, se o formulário for complexo, os usuários talvez não consigam preenchê-lo online, precisamos fornecer a capacidade de permitir que os usuários baixem a versão interativa do formulário a ser preenchido usando o Acrobat/Reader de maneira offline. Depois que o formulário for preenchido, o usuário poderá ficar online para enviar o formulário.
Para realizar esse caso de uso, precisamos executar as seguintes etapas:

* Capacidade de gerar PDF interativo/preenchível com os dados inseridos no formulário móvel
* Gerenciar o envio de PDF a partir do Acrobat/Reader
* Acione o fluxo de trabalho do Adobe Experience Manager (AEM) para revisar o PDF enviado

Este tutorial percorre as etapas necessárias para realizar o caso de uso acima. O código de amostra e os ativos relacionados a este tutorial estão [disponíveis aqui.](part-four.md)

O vídeo a seguir fornece uma visão geral do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

