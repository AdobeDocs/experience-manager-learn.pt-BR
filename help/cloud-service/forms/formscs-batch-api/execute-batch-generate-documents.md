---
title: Executar a configuração do lote
description: Inicie o processo de geração de documentos executando o lote
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# Executar configuração em lote

Para executar o lote, faça uma solicitação POST para a seguinte API

```xml
<baseURL>/confi/<configName>/execution
```

Essa API espera um objeto json vazio como parâmetro no corpo da solicitação.
Essa API retorna um URL exclusivo no cabeçalho de resposta identificado por **localização** chave.
Uma solicitação GET para esse URL exclusivo informará o status da execução do lote

O vídeo a seguir demonstra o acionamento da configuração do lote

>[!VIDEO](https://video.tv.adobe.com/v/340242/?quality=12&learn=on)
