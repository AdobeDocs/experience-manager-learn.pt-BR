---
title: Enviar anexos de formulário em um email
description: Extrair e enviar anexos de formulário enviados em um email usando fluxo de trabalho de automatização de energia
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Extrair anexos de formulário de dados de formulário enviados

Extraia anexos de formulário e envie os anexos em um e-mail em um fluxo de trabalho de automatização de energia.
O vídeo a seguir explica as etapas necessárias para formar anexos a partir dos dados enviados.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

A seguir está o schema de objetos de anexo que você precisa usar na etapa Analisar esquema JSON

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
