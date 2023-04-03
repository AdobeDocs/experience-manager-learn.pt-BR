---
title: Uso do compartilhamento de mídia social no AEM Sites
description: Explore a configuração e o uso do componente Compartilhamento de mídia social .
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 6%

---

# Uso do compartilhamento em mídia social {#using-social-media-sharing-in-aem-sites}

Explore a configuração e o uso do componente Compartilhamento de mídia social .

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Este vídeo explora os seguintes recursos do componente Compartilhamento de mídia social (parte de [Componentes principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)) usando o [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) site de exemplo.

* 0:00 - Adicionar e configurar o componente de Compartilhamento de mídia social
* 1:00 - Compartilhamento para o Facebook
* 3:10 - Compartilhamento para o Pinterest
* 6:25 - Uso do componente de Compartilhamento de mídia social em uma página de produto

## Configuração do Externalizador {#externalizer-setup}

![Externalizador de links CQ do dia](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizador](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) deve ser configurado no Autor do AEM e na Publicação do AEM, para mapear o modo de execução de publicação para o domínio acessível publicamente usado para acessar a Publicação do AEM.

Neste vídeo, usamos `/etc/hosts` ao parvo *www.example.com* para resolver para localhost e usar um [configuração básica do AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) para permitir que www.example.com coloque o AEM Publish na frente.

## Materiais de apoio {#supporting-materials}

* [Baixe os componentes principais do AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Baixar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalação do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
