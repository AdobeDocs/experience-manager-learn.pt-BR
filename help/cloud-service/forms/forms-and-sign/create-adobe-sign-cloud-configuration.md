---
title: Criar Cloud Service de configuração do Acrobat Sign Cloud
description: Crie a integração do AEM Forms e do Acrobat Sign usando a configuração dos serviços em nuvem.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 222
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---

# Criar configuração do Acrobat Sign Cloud

A configuração dos serviços em nuvem no AEM permite criar a integração entre o AEM e outros aplicativos em nuvem.

O vídeo a seguir o guiará pelas etapas necessárias para criar a configuração dos serviços em nuvem a fim de integrar o AEM ao Acrobat Sign

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Resolução de problemas

Se você receber um erro ao definir a configuração de nuvem do Adobe Sign, as seguintes etapas podem ser executadas para solucionar problemas
* Verifique se o URL de redirecionamento especificado no aplicativo da API do Acrobat Sign está no seguinte formato
&lt;your instance name>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Por exemplo - https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS é o nome do container que manterá a configuração de nuvem
* Verifique se o URL oAuth está correto
* Verifique a ID do cliente e o segredo do cliente
* Tentar modo de janela incógnito

