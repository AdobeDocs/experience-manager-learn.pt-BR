---
title: Configurar o BPA e o projeto CAM
description: Saiba como o Best Practice Analyzer e o Cloud Acceleration Manager fornecem um guia personalizado para a migração para AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# Analisador de práticas recomendadas e Cloud Acceleration Manager

Saiba como o Best Practice Analyzer (BPA) e o Cloud Acceleration Manager (CAM) fornecem um guia personalizado para migrar para AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Avaliar a prontidão

![Diagrama de alto nível de BPA e CAM](assets/bpa-cam-diagram.png)

O pacote BPA deve ser instalado em um clone do ambiente de produção AEM 6.x. O BPA gerará um relatório que poderá ser carregado para o CAM, o que fornecerá orientação sobre as atividades principais que precisam ocorrer para se mudarem para o AEM as a Cloud Service.

### Atividades principais

* Faça um clone do ambiente 6.x de produção. À medida que você migra o conteúdo e o código de refatoração, ter um clone de um ambiente de produção será valioso para testar várias ferramentas e alterações.
* Baixe a ferramenta BPA mais recente da [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) e instale no ambiente clonado do AEM 6.x.
* Use a ferramenta BPA para gerar um relatório que pode ser carregado para o Cloud Acceleration Manager (CAM). O CAM é acessado por meio de [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* Use CAM para fornecer orientação sobre quais atualizações precisam ser feitas na base de código e no ambiente atuais, a fim de mudar para AEM as a Cloud Service.
