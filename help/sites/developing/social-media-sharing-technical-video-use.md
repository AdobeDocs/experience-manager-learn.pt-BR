---
title: Utilização do compartilhamento de mídia social no AEM Sites
description: Explore a configuração e o uso do componente de Compartilhamento em redes sociais.
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 523
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# Uso de compartilhamento em redes sociais {#using-social-media-sharing-in-aem-sites}

Explore a configuração e o uso do componente de Compartilhamento em redes sociais.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Este vídeo explora os seguintes recursos do componente de Compartilhamento em redes sociais (parte da [Componentes principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)) utilizando o [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) site de exemplo.

* 0:00 - Adição e configuração do componente de Compartilhamento em redes sociais
* 1:00 - Compartilhamento no Facebook
* 3:10 - Compartilhamento no Pinterest
* 6:25 - Uso do componente de Compartilhamento em redes sociais em uma página de produto

## Configuração do externalizador {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[Externalizador de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) deve ser configurado no AEM Author e no AEM Publish, para mapear o modo de execução de publicação para o domínio de acesso público usado para acessar o AEM Publish.

Neste vídeo, usamos `/etc/hosts` para falsificação *www.example.com* para resolver para localhost, e use um [configuração básica do Dispatcher do AEM](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) para permitir que o www.example.com publique o AEM.

## Materiais de suporte {#supporting-materials}

* [Baixar os componentes principais do AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Baixar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalação do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
