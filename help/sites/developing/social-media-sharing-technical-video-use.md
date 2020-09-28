---
title: Uso do compartilhamento de mídia social no AEM Sites
description: Explore a configuração e o uso do componente Compartilhamento de mídia social.
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 7%

---


# Uso do compartilhamento de mídia social {#using-social-media-sharing-in-aem-sites}

Explore a configuração e o uso do componente Compartilhamento de mídia social.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Este vídeo explora os seguintes recursos do componente Compartilhamento de mídia social (parte de [AEM componentes](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)principais) usando o site de amostra [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) .

* 0:00 - Adicionar e configurar o componente de Compartilhamento de mídia social
* 1:00 - Compartilhamento no Facebook
* 3:10 - Compartilhamento no Pinterest
* 6:25 - Usar o componente de compartilhamento de mídia social em uma página do produto

## Configuração do Externalizador {#externalizer-setup}

![Externalizador de links CQ de dia](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizador](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) deve ser configurado no autor de AEM e no editor de AEM para mapear o modo de execução de publicação para o domínio acessível ao público usado para acessar o AEM Publish.

Neste vídeo, usamos `/etc/hosts` www.example.com *para resolver para localhost e usamos uma configuração* [](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) básica AEM Dispatcher para permitir que www.example.com seja aberto para publicar AEM.

## Materiais de suporte {#supporting-materials}

* [Download dos componentes principais AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Baixar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalação do Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
