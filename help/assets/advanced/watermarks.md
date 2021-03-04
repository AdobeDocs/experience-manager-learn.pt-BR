---
title: Marcas d'água no AEM Assets
description: Os recursos de marca d'água do AEM as a Cloud Service permitem que representações de imagem personalizadas sejam marcadas d'água usando qualquer imagem PNG.
feature: Microserviços do Asset Compute
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Gerenciamento de conteúdo
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# Marcas d&#39;água

Os recursos de marca d&#39;água do AEM as a Cloud Service permitem que representações de imagem personalizadas sejam marcadas d&#39;água usando qualquer imagem PNG.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## Configuração do OSGi

O seguinte stub de configuração OSGi pode ser atualizado e adicionado ao subprojeto `ui.config` do projeto AEM.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
