---
title: Modernização do repositório
description: Saiba mais sobre a modernização de repositórios, conteúdo mutável e imutável, estrutura de pacotes e a ferramenta de CLI do repositório modernizer.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8630
thumbnail: 336958.jpeg
exl-id: e9bd9035-1f2d-4a34-a581-9c1ec2c7bc04
duration: 1305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Modernização do repositório

Saiba mais sobre a modernização de repositórios, conteúdo mutável e imutável, estrutura de pacotes e a ferramenta de CLI do repositório modernizer.

>[!VIDEO](https://video.tv.adobe.com/v/3454797?quality=12&learn=on&captions=por_br)

## Ferramenta Modernizador de repositório

![Modernizador de repositório](./assets/repository-modernizer.png)

Como parte da refatoração de sua base de código, use a [ferramenta Repository Modernizer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/repo-modernizer.html?lang=pt-BR) para reestruturar uma base de código 6.x para uma estrutura mais moderna.

## Atividades principais

* Use a ferramenta [Modernizador de repositório do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationrepository-modernizer) para reestruturar um projeto para corresponder à estrutura esperada de um projeto do AEM as a Cloud Service.
* Ajuste e corrija manualmente quaisquer erros de build na base de código atualizada.
* Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implante a base de código atualizada. Repita até que o projeto esteja em um estado estável.
* Implante a base de código atualizada em um ambiente de desenvolvimento do AEM as a Cloud Service e continue a validar.
