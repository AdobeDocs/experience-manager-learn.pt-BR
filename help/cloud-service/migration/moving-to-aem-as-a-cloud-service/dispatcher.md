---
title: Configuração do Dispatcher ao mover-se para AEM as a Cloud Service
description: Saiba mais sobre alterações importantes no AEM Dispatcher para AEM as a Cloud Service, na ferramenta de conversão do Dispatcher e como usar o SDK de Ferramentas do Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

Saiba mais sobre AEM Dispatcher para AEM as a Cloud Service, com foco nas alterações importantes do Dispatcher para AEM 6, na ferramenta de conversão do Dispatcher e em como usar o SDK de Ferramentas do Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Conversor do Dispatcher Converter

![Conversor do Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Como parte da refatoração da base de código, use o [Conversor do Dispatcher do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) para refatorar as configurações existentes no local ou do Adobe Managed Services Dispatcher para AEM a configuração as a Cloud Service compatível do Dispatcher.

### Atividades principais

* Use o [Ferramenta Conversor do Dispatcher do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) para migrar uma configuração existente do Dispatcher.
* Faça referência ao módulo Dispatcher no [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) como prática recomendada.
* [Configurar ferramentas locais do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) para validar o dispatcher, antes de testar em um ambiente Cloud Service.


