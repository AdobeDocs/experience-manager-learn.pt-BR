---
title: Extensão das propriedades de página no AEM Sites
description: Saiba como estender os campos de metadados das Propriedades da página no Adobe Experience Manager Sites. Este vídeo detalha a maneira mais eficaz de fazer isso usando recursos do Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 492
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Extensão das propriedades da página {#extending-page-properties-in-aem-sites}

A personalização dos campos de metadados para as Propriedades da página é um requisito comum em qualquer implementação do Sites. Este vídeo detalha a maneira mais eficaz de fazer isso usando recursos do Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

O vídeo acima mostra como personalizar as propriedades da página para o [Site de referência da WKND](https://github.com/adobe/aem-guides-wknd).

## Exemplo de pacote de propriedades de página WKND

Você pode usar os [exemplo de pacote de propriedades de página WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contendo **WKND** e **Básico** personalizações de guias exibidas no vídeo acima. A variável **SocialMedia** a personalização de guias não é fornecida como [Componente da página WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) O agora usa a versão V3 dos Componentes principais do WCM e, na versão V3, o [o compartilhamento em redes sociais está obsoleto](https://github.com/adobe/aem-core-wcm-components/pull/1930).

No entanto, para fins de aprendizado, é possível direcionar o componente Página WKND para a versão V2 dos Componentes principais do WCM usando o `sling:resourceSuperType` valor da propriedade e sobrepor o [Redes sociais](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) guia. Para obter mais informações, consulte [Configuração das propriedades da página](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Este pacote de amostra deve ser instalado na instância local do AEM SDK ou AEM 6.X.X para fins de aprendizado.
