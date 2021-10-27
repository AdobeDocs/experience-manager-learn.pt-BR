---
title: Pesquisa e indexação em AEM as a Cloud Service
description: Saiba mais sobre AEM índices de pesquisa do as a Cloud Service, como converter AEM 6 definições de índice e como implantar índices.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Pesquisa e indexação

Saiba mais sobre AEM índices de pesquisa do as a Cloud Service, como converter AEM 6 definições de índice para serem AEM compatíveis e como implantar índices para AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Ferramenta Conversor de índice

![Ferramenta Conversor de índice](./assets/index-converter.png)

Como parte da refatoração da base de código, use o [Ferramenta conversor de índice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para converter definições de índice Oak personalizadas em AEM definições de índice compatíveis as a Cloud Service.

### Atividades principais

* Use o [Migrador de fluxo de trabalho do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) ferramenta para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços do Asset compute.
* Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) e implante os índices personalizados. Verifique se os índices atualizados estão atualizados.
* Implante a base de código atualizada em um ambiente de desenvolvimento as a Cloud Service AEM e continue a validar.
* Se modificar um índice predefinido **SEMPRE** copie a definição de índice mais recente de um ambiente AEM as a Cloud Service em execução na versão mais recente. Modifique a definição do índice copiado para atender às suas necessidades.