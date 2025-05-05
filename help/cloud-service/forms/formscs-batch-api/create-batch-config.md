---
title: Definir configuração de dados em lote
description: Definir configuração de dados em lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Criar configuração em lote

Para usar uma API de lote, crie uma configuração de lote e execute uma execução com base nessa configuração. O vídeo a seguir mostra uma demonstração da criação da configuração em lote usando a API

>[!NOTE]
>Certifique-se de que o usuário do AEM pertence ao grupo ```forms-users``` para fazer chamadas de API.


>[!VIDEO](https://video.tv.adobe.com/v/343703?quality=12&learn=on&captions=por_br)

## Criar configuração em lote

Este é o terminal POST para a criação da configuração de lote

```xml
<baseURL>/config
```

Esta é a configuração mínima que precisa ser especificada ao criar a configuração em lote. Isso precisa ser passado como objeto JSON no corpo da solicitação HTTP

```
{
    "configName": "monthlystatements",
    "dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
    "outputTypes": [
        "PDF"
    ],
    "template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## Verificar configuração de lote

Para verificar se a criação da configuração de lote foi bem-sucedida, é possível fazer uma chamada de solicitação do GET para o seguinte endpoint


```xml
<baseURL>/config/monthlystatements
```

Você só precisa passar um objeto JSON vazio no corpo da solicitação HTTP
