---
title: Implantar no ambiente de desenvolvimento
description: Implante o código a partir da ramificação do repositório do Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Implantar no ambiente de desenvolvimento

Na etapa anterior, enviamos a ramificação principal do repositório Git local para a ramificação MyFirstAF do repositório do Cloud Manager.

A próxima etapa é implantar o código no ambiente de desenvolvimento.
Faça logon no Cloud Manager e selecione seu programa

Selecione Implantar para desenvolvimento conforme mostrado abaixo


![primeira etapa](assets/deploy-first-step1.png)


Selecione Pipeline de implantação como mostrado
![primeira etapa](assets/deploy1.png)

Selecione o código-fonte e a ramificação Git apropriada
![primeira etapa](assets/deploy2.png)
Atualize suas alterações

Executar o pipeline
![run-pipeline](assets/run-pipeline.png)

Depois que o código for implantado, você deverá ver as alterações na instância do Cloud Service do AEM Forms.

## Próximas etapas

[Atualização do projeto do arquétipo maven](./updating-project-archetype.md)
