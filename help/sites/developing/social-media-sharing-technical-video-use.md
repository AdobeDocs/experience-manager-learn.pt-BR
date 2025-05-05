---
title: Utilização do compartilhamento de mídia social no AEM Sites
description: Explore a configuração e o uso do componente de Compartilhamento em redes sociais.
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# Uso de compartilhamento em redes sociais {#using-social-media-sharing-in-aem-sites}

Explore a configuração e o uso do componente de Compartilhamento em redes sociais.

>[!VIDEO](https://video.tv.adobe.com/v/30842?quality=12&learn=on&captions=por_br)

Este vídeo explora os seguintes recursos do componente de Compartilhamento em Redes Sociais (parte dos [Componentes principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)) usando o site de exemplo [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Adição e configuração do componente de Compartilhamento em redes sociais
* 1:00 - Compartilhamento no Facebook
* 3:10 - Compartilhamento no Pinterest
* 6:25 - Uso do componente de Compartilhamento em redes sociais em uma página de produto

## Configuração do externalizador {#externalizer-setup}

![Externalizador de links CQ de dias](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

O [externalizador do AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/externalizer.html) deve ser configurado no AEM Author e no AEM Publish, para mapear o modo de execução de publicação para o domínio de acesso público usado para acessar o AEM Publish.

Neste vídeo, usamos `/etc/hosts` para falsificar *www.example.com* para resolver para localhost e usamos uma [configuração básica do AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=pt-BR) para permitir que o www.example.com mostre o AEM Publish.

## Materiais de suporte {#supporting-materials}

* [Baixar os Componentes principais do AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Baixar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalação do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=pt-BR)
