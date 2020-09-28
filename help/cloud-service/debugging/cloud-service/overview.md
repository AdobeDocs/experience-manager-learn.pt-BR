---
title: Depuração de AEM como Cloud Service
description: em infraestrutura de autoatendimento, dimensionável e em nuvem, o que torna necessário que os desenvolvedores AEM entendam como entender e depurar várias facetas de AEM como Cloud Service, da criação e implantação à obtenção de detalhes sobre a execução de aplicativos AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# Depuração de AEM como Cloud Service

A AEM como Cloud Service é a forma nativa da nuvem de aproveitar os aplicativos AEM. AEM como um Cloud Service é executado em infraestrutura de nuvem de autoatendimento, escalável e escalonável, o que requer que os desenvolvedores AEM entendam como entender e depurar várias facetas de AEM como Cloud Service, da criação e implantação à obtenção de detalhes sobre a execução de aplicativos AEM.

## Logs

Os registros fornecem detalhes sobre como seu aplicativo está funcionando no AEM como um Cloud Service, bem como insights sobre problemas com implantações.

[Depuração de AEM como Cloud Service usando registros](./logs.md)

## Criar e implantar

Os pipelines do Adobe Cloud Manager implantam AEM aplicativo por meio de uma série de etapas para determinar a qualidade e viabilidade do código quando implantados em AEM como Cloud Service. Cada uma das etapas pode resultar em falha, tornando importante entender como depurar compilações para determinar a causa raiz de e como resolver qualquer falha.

[Depuração de AEM como criação e implantação de Cloud Service](./build-and-deployment.md)

## Console do desenvolvedor

O console do Desenvolvedor fornece várias informações e introspecções no AEM como ambientes Cloud Service que são úteis para entender como seu aplicativo é reconhecido e funciona dentro do AEM como um Cloud Service.

[Depuração de AEM como Cloud Service com o Developer Console](./developer-console.md)

## CRXDE Lite

O CRXDE Lite é uma ferramenta clássica, mas poderosa, para depurar AEM como ambientes de desenvolvimento de Cloud Service. O CRXDE Lite fornece um conjunto de funcionalidades que ajuda a depuração a inspecionar todos os recursos e propriedades, manipular as partes mutáveis do JCR, investigar permissões e avaliar query.

[Depuração de AEM como um Cloud Service com CRXDE Lite](./crxde-lite.md)
