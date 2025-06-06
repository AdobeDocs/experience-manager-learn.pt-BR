---
title: Integrar tags na Adobe Experience Platform e no AEM
description: As tags na coleção de dados da Experience Platform são a solução de gerenciamento de tags de última geração da Adobe e a melhor maneira de implantar o Adobe Analytics, o Target, o Audience Manager e várias outras soluções. Confira uma visão geral das tags na Adobe Experience Platform e a integração recomendada com o Adobe Experience Manager.
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
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Integrar tags da coleção de dados da Experience Platform e AEM {#overview}

Saiba como integrar tags na Adobe Experience Platform com o Adobe Experience Manager.

As tags são a próxima geração da tecnologia de gerenciamento de tags da Adobe Experience Platform. As tags são a maneira mais simples de implantar o Adobe Analytics, o Target, o Audience Manager e várias outras soluções. Confira uma visão geral das tags e a integração recomendada com o Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3445205?quality=12&learn=on&captions=por_br)

## Pré-requisitos

Os itens a seguir são obrigatórios ao integrar tags da coleção de dados da Experience Platform.

+ Acesso do administrador do AEM ao ambiente do AEM as a Cloud Service
+ Um site de referência como o da [WKND](https://github.com/adobe/aem-guides-wknd) foi implantado nele.
+ Acesso à solução de coleção de dados da Adobe Experience Platform
+ Acesso do administrador do sistema ao [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## Etapas de nível superior

+ Na coleção de dados da Adobe Experience Platform, crie uma propriedade de tag e edite-a para _Adicionar regra_. Em seguida, em _Adicionar biblioteca_, selecione a regra recém-adicionada, aprove-a e publique-a.
+ Conecte o AEM às tags por meio da configuração do IMS existente (ou nova)
+ No AEM, crie uma configuração de serviço na nuvem de tags, aplique-a a um site existente e, por fim, verifique se a propriedade de tags e suas bibliotecas são carregadas no site publicado ou do criador.

## Próximas etapas

[Criar uma propriedade de tag](create-tag-property.md)

## Recursos adicionais {#additional-resources}

+ [Integrações da Experience Platform com aplicativos da Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html?lang=pt-BR)
+ [Visão geral das tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=pt-BR)
+ [Implementar a Experience Cloud em sites com tags](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html?lang=pt-BR)
