---
title: Integração das Tags de coleta de dados do Experience Platform (Launch) e do AEM
description: A Coleta de dados do Tags é a solução de gerenciamento de tags da próxima geração do Adobe e a melhor maneira de implantar o Adobe Analytics, o Target, o Audience Manager e muitas outras soluções. Obtenha uma visão geral das Tags (anteriormente conhecidas como Launch) e a integração recomendada com o Adobe Experience Manager.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Integração de Tags e AEM de coleta de dados do Experience Platform {#overview}

Saiba como integrar o Experience Platform _Tags de coleta de dados_ (anteriormente conhecido como Launch) com o Adobe Experience Manager.

>[!NOTE]
>
>A Adobe Experience Platform Launch foi reformulada como um conjunto de tecnologias de coleta de dados no Adobe Experience Platform. Como resultado, várias alterações de terminologia foram implementadas na documentação do produto. Consulte o seguinte [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para uma referência consolidada das alterações de terminologia.


Tags são a próxima geração da tecnologia de gerenciamento de tags da Adobe Experience Platform. As tags fornecem a maneira mais simples de implantar o Adobe Analytics, o Target, o Audience Manager e muitas outras soluções. Obtenha uma visão geral das Tags e a integração recomendada com o Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Pré-requisitos

Os itens a seguir são necessários ao integrar Tags de coleta de dados do Experience Platform.

+ AEM acesso do administrador a AEM ambiente as a Cloud Service
+ Um site de referência como [WKND](https://github.com/adobe/aem-guides-wknd) implantado nele.
+ Acesso à solução Adobe Experience Platform Data Collection
+ Acesso do administrador do sistema para [Console do Adobe Developer](https://developer.adobe.com/developer-console/)


## Etapas de alto nível

+ Na Coleta de dados do Adobe Experience Platform, crie uma propriedade de tag e edite-a em _Adicionar regra_. Então _Adicionar biblioteca_, selecione a regra adicionada recentemente, aprove e publique-a.
+ Conecte AEM e tags usando a configuração IMS existente (ou nova)
+ Em AEM, crie uma configuração de serviços em nuvem do Launch e, em seguida, aplique-a a um site existente e, por fim, verifique se a propriedade Tags e suas bibliotecas estão carregadas no site Publicado ou Autor .

## Próximas etapas

[Criar uma propriedade de tag](create-tag-property.md)

## Recursos adicionais {#additional-resources}

+ [Integrações do Experience Platform com aplicativos Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Visão geral das tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementação do Experience Cloud em sites com tags](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)