---
title: Geração de documentos usando API em lote no AEM Forms CS
description: Configure e acione operações em lote para gerar documentos.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Introdução

Uma solicitação em lote é onde dezenas, centenas ou milhares de documentos semelhantes são gerados de uma vez. Exemplo: Uma empresa financeira pode gerar declarações de cartão de crédito para enviar a todos os seus clientes.
As APIs em lote (APIs assíncronas) são adequadas para casos de uso de geração de documento com várias throughput programada. Essas APIs geram documentos em lotes. Por exemplo, contas telefônicas, demonstrativos de cartão de crédito e demonstrativos de benefícios gerados todo mês.

Para usar a API de operação em lote do AEM Forms CS, as seguintes configurações são necessárias

1. Configurar conta de armazenamento do Azure
1. Criar configuração de nuvem com base em armazenamento do Azure
1. Criar configuração de armazenamento de dados em lote
1. Executar a API em lote

Recomendamos que você se familiarize com o [Documentação da API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) antes de continuar a usar este tutorial.
