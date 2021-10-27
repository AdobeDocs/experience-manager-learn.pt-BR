---
title: Microserviços AEM Assets e mudança para AEM as a Cloud Service
description: Saiba como os microsserviços de asset compute do AEM Assets as a Cloud Service permitem que você gere de forma automática e eficiente qualquer representação dos seus ativos, substituindo essa função do fluxo de trabalho AEM tradicional.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# Microserviços AEM Assets - Migração para AEM as a Cloud Service

Saiba como os microsserviços de asset compute do AEM Assets as a Cloud Service permitem que você gere de forma automática e eficiente qualquer representação dos seus ativos, substituindo essa função do fluxo de trabalho AEM tradicional.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Ferramenta Migração de fluxo de trabalho

![Ferramenta Migração de fluxo de trabalho de ativos](./assets/asset-workflow-migration.png)

Como parte da refatoração da base de código, use o [Ferramenta Migração de fluxo de trabalho de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) para migrar workflows existentes para usar os microsserviços do Asset compute em AEM as a Cloud Service.

### Atividades principais

* Use o [Migrador de fluxo de trabalho do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) ferramenta para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços do Asset compute.
* Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e implante os workflows atualizados. Pode ser necessário um ajuste manual para fluxos de trabalho complexos.
* Continue a iterar em um ambiente de desenvolvimento local usando o SDK do AEM até que o fluxo de trabalho atualizado corresponda à paridade de recursos.
* Implante a base de código atualizada em um ambiente de desenvolvimento as a Cloud Service AEM e continue a validar.

