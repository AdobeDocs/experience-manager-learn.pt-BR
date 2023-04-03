---
title: Extensão das propriedades da página no AEM Sites
description: Saiba como estender os campos de metadados das Propriedades da página no Adobe Experience Manager Sites. Este vídeo detalha a maneira mais eficaz de fazer isso usando os recursos do Sling Resource Merger .
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# Extensão das propriedades da página {#extending-page-properties-in-aem-sites}

Personalizar os campos de metadados para as Propriedades da página é um requisito comum em qualquer implementação do Sites. Este vídeo detalha a maneira mais eficaz de fazer isso usando os recursos do Sling Resource Merger .

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

O vídeo acima mostra como personalizar as propriedades da página para o [Site de referência WKND](https://github.com/adobe/aem-guides-wknd).

## Exemplo de pacote de propriedades da página WKND

Você pode usar o [exemplo de pacote de propriedades da página WKND](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) contendo **WKND** e **Básico** personalizações de guias mostradas no vídeo acima. O **SocialMedia** a personalização da guia não é fornecida como [Componente de página WKND](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) agora usa a versão V3 dos Componentes principais do WCM e, na versão V3, a variável [o compartilhamento em redes sociais está obsoleto](https://github.com/adobe/aem-core-wcm-components/pull/1930).

No entanto, para fins de aprendizado, você pode apontar o componente Página WKND para a versão V2 dos Componentes principais do WCM usando o `sling:resourceSuperType` e sobreponha o [Redes sociais](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) guia . Para obter mais informações, consulte [Configurar as propriedades da página](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Este pacote de amostra deve ser instalado no SDK de AEM local ou AEM instância 6.X.X para fins de aprendizado.
