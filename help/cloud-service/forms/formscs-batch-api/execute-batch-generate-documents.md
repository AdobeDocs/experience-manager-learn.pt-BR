---
title: Executar a configuração do lote
description: Iniciar o processo de geração de documentos executando o lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Executar configuração de lote

Para executar o lote, faça uma solicitação POST para a seguinte API

```xml
<baseURL>/confi/<configName>/execution
```

Essa API espera um objeto json vazio como parâmetro no corpo da solicitação.
Essa API retorna um URL exclusivo no cabeçalho de resposta identificado por **localização** chave.
Uma solicitação de GET para esse URL exclusivo informará o status da execução do lote

O vídeo a seguir demonstra o acionamento da configuração do lote

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
