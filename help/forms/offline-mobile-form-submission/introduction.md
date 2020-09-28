---
title: Acionar fluxo de trabalho AEM no envio de formulário HTM5
seo-title: Acionar fluxo de trabalho AEM no envio de formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# Download de formulário móvel parcialmente concluído e envio para AEM fluxo de trabalho

Um caso de uso comum é ter a capacidade de renderizar o XDP como HTML para atividades de captura de dados. Isso funciona bem quando os formulários são simples e podem ser preenchidos e enviados online. No entanto, se o formulário for complexo, os usuários talvez não consigam preenchê-lo online, será necessário fornecer a capacidade de permitir que os usuários baixem a versão interativa do formulário a ser preenchido usando o Acrobat/Reader offline. Depois que o formulário for preenchido, o usuário poderá estar on-line para enviar o formulário.
Para realizar esse caso de uso, é necessário executar as seguintes etapas:

* Capacidade de gerar PDF interativo/preenchível com os dados inseridos no formulário móvel
* Processar o envio de PDF da Acrobat/Reader
* Acionar o fluxo de trabalho do Adobe Experience Manager (AEM) para revisar o PDF enviado

Este tutorial percorre as etapas necessárias para obter o caso de uso acima. O código de amostra e os ativos relacionados a este tutorial estão [disponíveis aqui.](part-four.md)

O vídeo a seguir fornece uma visão geral do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

