---
title: Depuração de AEM como Cloud Service
description: na infraestrutura em nuvem de autoatendimento, dimensionável e dimensionável, que torna o exige que os desenvolvedores de AEM entendam como entender e depurar várias facetas do AEM as a Cloud Service, desde a criação e implantação até obter detalhes sobre a execução de aplicativos AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---

# Depuração de AEM como Cloud Service

O AEM como Cloud Service é a forma nativa em nuvem de aproveitar os aplicativos AEM. O AEM as a Cloud Service é executado em infraestrutura em nuvem de autoatendimento, escalável e dimensionável, o que requer que os desenvolvedores de AEM entendam como entender e depurar várias facetas do AEM como um Cloud Service, desde a criação e implantação até obter detalhes da execução de aplicativos AEM.

## Logs

Os registros fornecem detalhes sobre como seu aplicativo está funcionando no AEM como um Cloud Service, bem como insights sobre problemas com implantações.

[Depuração de AEM como Cloud Service usando logs](./logs.md)

## Criar e implantar

Os pipelines do Adobe Cloud Manager implantam AEM aplicativo por meio de uma série de etapas para determinar a qualidade e a viabilidade do código quando implantados no AEM como um Cloud Service. Cada uma das etapas pode resultar em falha, tornando importante entender como depurar builds para determinar a causa raiz de e como resolver qualquer falha.

[Depuração de AEM como criação e implantação do Cloud Service](./build-and-deployment.md)

## Console do desenvolvedor

O Console do desenvolvedor fornece uma variedade de informações e introduções no AEM como ambientes de Cloud Service, úteis para entender como seu aplicativo é reconhecido pelo e funciona no AEM como um Cloud Service.

[Depuração do AEM como um Cloud Service com o Console do desenvolvedor](./developer-console.md)

## CRXDE Lite

O CRXDE Lite é uma ferramenta clássica, mas poderosa, para depurar AEM como ambientes de desenvolvimento de Cloud Service. O CRXDE Lite fornece um conjunto de funcionalidades que auxilia a depuração a inspecionar todos os recursos e propriedades, manipular as partes mutáveis do JCR, investigar permissões e avaliar consultas.

[Depuração de AEM como um Cloud Service com CRXDE Lite](./crxde-lite.md)
