---
title: Executar a configuração do lote
description: Iniciar o processo de geração de documentos executando o lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# Executar configuração de lote

Para executar o lote, faça uma solicitação POST para a seguinte API

```xml
<baseURL>/confi/<configName>/execution
```

Essa API espera um objeto json vazio como parâmetro no corpo da solicitação.
Esta API retorna uma URL exclusiva no cabeçalho de resposta identificado pela chave **location**.
Uma solicitação GET para esse URL exclusivo informará o status da execução do lote

O vídeo a seguir demonstra o acionamento da configuração do lote

>[!VIDEO](https://video.tv.adobe.com/v/343710?quality=12&learn=on&captions=por_br)
