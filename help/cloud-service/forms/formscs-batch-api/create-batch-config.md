---
title: Definir configuração de dados em lote
description: Definir configuração de dados em lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 252
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Criar configuração em lote

Para usar uma API de lote, crie uma configuração de lote e execute uma execução com base nessa configuração. O vídeo a seguir mostra uma demonstração da criação da configuração em lote usando a API

>[!NOTE]
>Certifique-se de que o usuário do AEM pertence a ```forms-users``` grupo para fazer chamadas de API.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## Criar configuração em lote

Este é o endpoint de POST para criação da configuração de Lote

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
