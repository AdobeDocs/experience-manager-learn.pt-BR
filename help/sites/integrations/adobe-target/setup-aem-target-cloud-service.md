---
title: Criar conta do Adobe Target Cloud Service no AEM
description: Integre o Adobe Experience Manager as a Cloud Service ao Adobe Target usando a autenticação do Cloud Service e do Adobe IMS.
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 8ebaba01d4b470a1ca7c1630f55b756796f3640d
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 8%

---

# Criar conta do Adobe Target Cloud Service {#adobe-target-cloud-service}

O vídeo a seguir fornece uma apresentação sobre como conectar o AEM as a Cloud Service ao Adobe Target.

Essa integração permite que o serviço do Autor do AEM se comunique diretamente com o Adobe Target e envie fragmentos de experiência do AEM para o Target como ofertas.  Esta integração *não* adiciona o Adobe Target JavaScript (AT.js) às páginas da Web do AEM Sites, para essa integração [AEM e tags usando a extensão do Target](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md).

>[!WARNING]
>
>O vídeo mostra um método de autenticação JWT obsoleto para conectar o AEM ao Adobe Target. No entanto, o método recomendado é usar o método de autenticação de servidor para servidor OAuth. Para obter mais informações, consulte [Migração de credenciais JWT para OAuth para o AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration). Estamos trabalhando na atualização do vídeo para refletir essa mudança.


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Há um problema conhecido com a configuração do Adobe Target Cloud Services mostrado no vídeo. Até que esse problema seja resolvido, siga as mesmas etapas no vídeo, mas use a [configuração herdada do Adobe Target Cloud Services](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html?lang=pt-BR).
