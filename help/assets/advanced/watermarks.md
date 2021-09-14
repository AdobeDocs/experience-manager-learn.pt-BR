---
title: Marcas d'água no AEM Assets
description: AEM como um recurso de marca d'água de Cloud Service permite que imagens personalizadas sejam marcadas d'água usando qualquer imagem PNG.
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# Marcas d&#39;água

AEM como um recurso de marca d&#39;água de Cloud Service permite que imagens personalizadas sejam marcadas d&#39;água usando qualquer imagem PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configuração do OSGi

O seguinte stub de configuração do OSGi pode ser atualizado e adicionado ao subprojeto `ui.config` do projeto AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
