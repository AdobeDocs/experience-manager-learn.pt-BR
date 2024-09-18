---
title: Acionar fluxo de trabalho de AEM na introdução do envio do formulário HTML5
description: Acionar um fluxo de trabalho de AEM no envio de formulários para dispositivos móveis
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# Acionar o fluxo de trabalho do AEM no envio de um formulário para dispositivos móveis

Um caso de uso comum é a capacidade de renderizar o XDP como HTML para atividades de captura de dados.No envio deste formulário, pode haver a necessidade de acionar um workflow AEM. No fluxo de trabalho do AEM, podemos unir os dados ao modelo xdp e apresentar o PDF gerado para revisão e aprovação. O formulário será renderizado em uma instância de publicação e o fluxo de trabalho será acionado em uma instância de processamento do AEM.
As etapas a seguir estão envolvidas no caso de uso

* O usuário preenche e envia um formulário HTML5 (representação HTML5 de XDP).
* A submissão é tratada por um servlet na instância de publicação.
* O servlet armazena os dados em uma pasta predefinida no repositório da instância de processamento do AEM.
* O iniciador de fluxos de trabalho é configurado para acionar um fluxo de trabalho AEM toda vez que um novo arquivo é adicionado em uma pasta específica.

Este tutorial aborda as etapas necessárias para realizar o caso de uso acima. Exemplos de código e ativos relacionados a este tutorial estão [disponíveis aqui.](./deploy-assets.md)


## Próximas etapas

[Tratar Envio De Formulário](./handle-form-submission.md)