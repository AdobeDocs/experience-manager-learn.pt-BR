---
title: Extensão das propriedades de página no AEM Sites
description: Saiba como estender os campos de metadados das Propriedades da página no Adobe Experience Manager Sites. Este vídeo detalha a maneira mais eficaz de fazer isso usando recursos do Sling Resource Merger.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Extensão das propriedades da página {#extending-page-properties-in-aem-sites}

A personalização dos campos de metadados para as Propriedades da página é um requisito comum em qualquer implementação do Sites. Este vídeo detalha a maneira mais eficaz de fazer isso usando recursos do Sling Resource Merger.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

O vídeo acima mostra como personalizar as propriedades de página do [Site de Referência WKND](https://github.com/adobe/aem-guides-wknd).

## Exemplo de pacote de propriedades de página WKND

Você pode usar o [pacote de propriedades de página WKND de amostra](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) fornecido contendo as personalizações de guia **WKND** e **Básicas** mostradas no vídeo acima. A personalização da guia **SocialMedia** não é fornecida porque o [componente da Página do WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) agora usa a versão V3 dos Componentes Principais do WCM e, na versão V3, o [compartilhamento em redes sociais foi descontinuado](https://github.com/adobe/aem-core-wcm-components/pull/1930).

No entanto, para fins de aprendizado, você pode apontar o componente Página WKND para a versão V2 dos Componentes principais WCM usando o valor da propriedade `sling:resourceSuperType` e sobrepor a guia [Redes sociais](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95). Para obter mais informações, consulte [Configurando suas Propriedades de Página](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=pt-BR#configuring-your-page-properties)

Este pacote de amostra deve ser instalado na instância local do AEM SDK ou do AEM 6.X.X para fins de aprendizado.
