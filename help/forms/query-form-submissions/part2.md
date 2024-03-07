---
title: Consulta ao envio de formulário
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas na consulta de envios de formulários armazenados no portal do Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 1%

---

# Criar envio personalizado

Um manipulador de envio personalizado foi criado para lidar com o envio do formulário. Em um nível superior, o manipulador de envio personalizado faz o seguinte

* Extrai o nome do formulário enviado.
* Extrai os dados enviados. Os dados enviados de um formulário baseado em componente principal estão sempre no formato JSON.
* Extrai e armazena os anexos de formulário no portal do Azure. Atualiza os dados json enviados com o url do anexo.
* Cria tags de índice de blob - Localiza a lista de campos pesquisáveis para o formulário e seu valor correspondente dos dados enviados.
* Associa as tags de índice de blob aos dados enviados e os armazena no portal do Azure.

A captura de tela a seguir mostra as tags de índice de blob no portal do Azure

![blob-index-tags](assets/blob-index-tags.png)

O código de envio personalizado está em **_StoreFormDataWithBlobIndexTagsInAzure_** e o código para armazenar e recuperar dados do Azure está no componente **_SaveAndFetchFromAzure_**

## Próximas etapas

[Criar interface de consulta](./part3.md)

