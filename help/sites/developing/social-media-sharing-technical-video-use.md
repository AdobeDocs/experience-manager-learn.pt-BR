---
title: Uso do compartilhamento de mídia social no AEM Sites
description: Explore a configuração e o uso do componente Compartilhamento de mídia social .
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 9%

---


# Uso do compartilhamento de mídia social {#using-social-media-sharing-in-aem-sites}

Explore a configuração e o uso do componente Compartilhamento de mídia social .

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Este vídeo explora os seguintes recursos do componente Compartilhamento de mídia social (parte de [Componentes principais do AEM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)) usando o [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) site de amostra.

* 0:00 - Adicionar e configurar o componente de Compartilhamento de mídia social
* 1:00 - Compartilhamento no Facebook
* 3:10 - Compartilhamento para o Pinterest
* 6:25 - Uso do componente de Compartilhamento de mídia social em uma página de produto

## Configuração do Externalizador {#externalizer-setup}

![Externalizador de links CQ do dia](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[O ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externalizador do AEM deve ser configurado no Autor do AEM e na Publicação do AEM, para mapear o modo de execução de publicação para o domínio acessível publicamente usado para acessar a Publicação do AEM.

Neste vídeo, usamos `/etc/hosts` para falsificar *www.example.com* para resolver para localhost, e usamos uma [configuração básica do AEM Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) para permitir que www.example.com coloque a publicação do AEM na frente.

## Materiais de suporte {#supporting-materials}

* [Baixar os Componentes principais do AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Baixar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalação do Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
