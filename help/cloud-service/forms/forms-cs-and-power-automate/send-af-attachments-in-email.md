---
title: Enviar anexos de formulário em um email
description: Extrair e enviar anexos de formulário enviados em um email usando o workflow do Power Automate
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="Cloud Service AEM Forms" before-title="false"
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '76'
ht-degree: 0%

---

# Extrair anexos de formulário de dados de formulário enviados

Extraia anexos de formulário e envie-os por email no fluxo de trabalho do Power Automate.
O vídeo a seguir explica as etapas necessárias para formar anexos a partir dos dados enviados.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

A seguir está o esquema de objeto de anexo que você precisa usar na etapa Analisar esquema JSON

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
