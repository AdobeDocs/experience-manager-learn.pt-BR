---
title: Integração de tags no Adobe Experience Platform e no AEM
description: As tags na coleção de dados do Experience Platform Adobe são a solução de gerenciamento de tags de próxima geração e a melhor maneira de implantar o Adobe Analytics, o Target, o Audience Manager e muitas outras soluções. Obtenha uma visão geral das tags na Adobe Experience Platform e a integração recomendada com o Adobe Experience Manager.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 256
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 2%

---

# Integração de tags de coleção de dados do Experience Platform e AEM {#overview}

Saiba como integrar as tags na Adobe Experience Platform com o Adobe Experience Manager.

As tags são a próxima geração da tecnologia de gerenciamento de tags da Adobe Experience Platform. As tags oferecem a maneira mais simples de implantar o Adobe Analytics, o Target, o Audience Manager e muitas outras soluções. Obtenha uma visão geral das Tags e a integração recomendada com o Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## Pré-requisitos

Os itens a seguir são obrigatórios ao integrar as Tags da coleção de dados do Experience Platform.

+ Acesso do administrador do AEM ao ambiente as a Cloud Service do AEM
+ Um site de referência como [WKND](https://github.com/adobe/aem-guides-wknd) implantado nela.
+ Acesso à solução de coleta de dados da Adobe Experience Platform
+ Acesso de administrador do sistema a [Console do Adobe Developer](https://developer.adobe.com/developer-console/)


## Etapas de alto nível

+ Na Coleção de dados do Adobe Experience Platform, crie uma propriedade de tag e edite-a para _Adicionar regra_. Depois _Adicionar biblioteca_, selecione a regra recém-adicionada, aprove e publique-a.
+ Conecte AEM e tags usando a configuração IMS existente (ou nova)
+ No AEM, crie uma configuração de serviço na nuvem de tags, aplique-a a um site existente e, por fim, verifique se a propriedade Tags e suas bibliotecas são carregadas no site Publicado ou do Autor.

## Próximas etapas

[Criar uma propriedade de tag](create-tag-property.md)

## Recursos adicionais {#additional-resources}

+ [Integrações de Experience Platform com aplicativos de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Visão geral das tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementar o Experience Cloud em sites com tags](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
