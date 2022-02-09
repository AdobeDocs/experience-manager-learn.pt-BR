---
title: Configurar configuração de dados em lote
description: Configurar configuração de dados em lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Criar configuração de lote

Para usar uma API em lote, crie uma configuração em lote e execute uma execução com base nessa configuração. O vídeo a seguir mostra uma demonstração da criação da configuração de lote usando a API

>[!NOTE]
>Certifique-se de que o usuário AEM pertence a ```forms-users``` para fazer chamadas de API.


>[!VIDEO](https://video.tv.adobe.com/v/340241/?quality=12&learn=on)

## Criar configuração em lote

Este é o ponto de extremidade do POST para criar a configuração em lote

```xml
<baseURL>/config
```

A seguir encontra-se a configuração mínima que precisa ser especificada ao criar a configuração de lote. Isso precisa ser passado como objeto JSON no corpo da solicitação HTTP

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

## Verificar configuração em lote

Para verificar a criação bem-sucedida da configuração de lote, você pode fazer uma chamada de solicitação de GET para o seguinte endpoint


```xml
<baseURL>/config/monthlystatements
```

Você só precisa transmitir um objeto JSON vazio no corpo da solicitação HTTP

