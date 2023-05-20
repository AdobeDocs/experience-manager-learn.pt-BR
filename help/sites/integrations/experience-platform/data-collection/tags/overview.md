---
title: Integração de tags de coleção de dados do Experience Platform (Launch) e AEM
description: As tags na coleção de dados do Experience Platform Adobe são a solução de gerenciamento de tags de próxima geração e a melhor maneira de implantar o Adobe Analytics, o Target, o Audience Manager e muitas outras soluções. Obtenha uma visão geral de Tags (antigo Launch) e a integração recomendada com o Adobe Experience Manager.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Integração de tags de coleção de dados do Experience Platform e AEM {#overview}

Saiba como integrar o Experience Platform _Tags de coleção de dados_ (conhecido anteriormente como Launch) com o Adobe Experience Manager.

>[!NOTE]
>
>O Adobe Experience Platform Launch foi reformulado como um conjunto de tecnologias de coleção de dados na Adobe Experience Platform. Como resultado, várias alterações de terminologia foram implementadas na documentação do produto. Consulte o seguinte [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para obter uma referência consolidada das alterações de terminologia.


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
+ No AEM, crie uma configuração do Launch Cloud Services, aplique-a a um site existente e, por fim, verifique se a propriedade Tags e suas bibliotecas são carregadas no site Publicado ou do Autor.

## Próximas etapas

[Criar uma propriedade de tag](create-tag-property.md)

## Recursos adicionais {#additional-resources}

+ [Integrações de Experience Platform com aplicativos de Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Visão geral das tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Implementar o Experience Cloud em sites com tags](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
