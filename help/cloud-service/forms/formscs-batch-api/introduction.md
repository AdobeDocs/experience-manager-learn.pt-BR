---
title: Geração de documentos usando a API em lote no AEM Forms CS
description: Configure e acione operações em lote para gerar documentos.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 14%

---

# Introdução

Uma solicitação em lote é onde dezenas, centenas ou milhares de documentos semelhantes são gerados ao mesmo tempo. Exemplo: uma empresa financeira pode gerar demonstrativos de cartão de crédito para enviar a todos os seus clientes.
As APIs em lote (APIs assíncronas) são adequadas para casos de uso programados de alta taxa de transferência na geração de vários documentos. Essas APIs geram documentos em lotes. Por exemplo, contas telefônicas, demonstrativos de cartão de crédito e demonstrativos de benefícios gerados todo mês.

Para usar a API de operação em lote do AEM Forms CS, as seguintes configurações são necessárias

1. Configurar conta de armazenamento do Azure
1. Criar configuração de nuvem com suporte de armazenamento do Azure
1. Criar configuração do armazenamento de dados em lote
1. Executar a API em lote

É recomendável que você se familiarize com o [Documentação da API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) antes de continuar a usar este tutorial.
