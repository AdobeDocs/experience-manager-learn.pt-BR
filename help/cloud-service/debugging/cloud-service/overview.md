---
title: Depuração do AEM as a Cloud Service
description: na infraestrutura em nuvem dimensionável, de autoatendimento, que torna o exige que os desenvolvedores do AEM entendam como entender e depurar várias facetas do AEM as a Cloud Service, desde a criação e a implantação até obter detalhes sobre a execução de aplicativos do AEM.
feature: Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante, Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 2%

---


# Depuração do AEM as a Cloud Service

O AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos do AEM. O AEM as a Cloud Service é executado em infraestrutura em nuvem de autoatendimento, dimensionável e dimensionável, o que requer que os desenvolvedores do AEM entendam e depurem várias facetas do AEM as a Cloud Service, da criação e implantação à obtenção de detalhes da execução de aplicativos do AEM.

## Logs

Os registros fornecem detalhes sobre como seu aplicativo está funcionando no AEM as a Cloud Service, bem como insights de problemas com implantações.

[Depuração do AEM as a Cloud Service usando logs](./logs.md)

## Criar e implantar

Os pipelines do Adobe Cloud Manager implantam o aplicativo AEM por meio de uma série de etapas para determinar a qualidade e a viabilidade do código quando implantados no AEM as a Cloud Service. Cada uma das etapas pode resultar em falha, tornando importante entender como depurar builds para determinar a causa raiz de e como resolver qualquer falha.

[Depuração da build e implantação do AEM as a Cloud Service](./build-and-deployment.md)

## Console do desenvolvedor

O Console do desenvolvedor fornece uma variedade de informações e introduções nos ambientes do AEM as a Cloud Service, úteis para entender como seu aplicativo é reconhecido pelo e funciona no AEM as a Cloud Service.

[Depuração do AEM as a Cloud Service com o Console do desenvolvedor](./developer-console.md)

## CRXDE Lite

O CRXDE Lite é uma ferramenta clássica, mas poderosa, para depurar ambientes de desenvolvimento do AEM as a Cloud Service. O CRXDE Lite fornece um conjunto de funcionalidades que auxilia a depuração a inspecionar todos os recursos e propriedades, manipular as partes mutáveis do JCR, investigar permissões e avaliar consultas.

[Depuração do AEM as a Cloud Service com o CRXDE Lite](./crxde-lite.md)
