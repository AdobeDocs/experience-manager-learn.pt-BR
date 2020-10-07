---
title: Marcas d'água no AEM Assets
description: AEM como um recurso de marcação d'água de representação Cloud Service permite que a imagem personalizada tenha uma marca d'água usando qualquer imagem PNG.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# Marcas d&#39;água

AEM como um recurso de marcação d&#39;água de representação Cloud Service permite que a imagem personalizada tenha uma marca d&#39;água usando qualquer imagem PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configuração do OSGi

O seguinte stub de configuração OSGi pode ser atualizado e adicionado ao `ui.config` subprojeto do projeto AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
