---
title: Criar o formulário inicial para acionar o processo
description: Crie um formulário inicial para acionar a notificação por email para iniciar o processo de assinatura.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# Criar formulário inicial

O formulário inicial (Formulário de refinanciamento) é usado para assinar vários formulários, acionando o **Assinar várias Forms** AEM fluxo de trabalho. Você pode inserir valores de sua escolha, mas garantir que os seguintes campos sejam adicionados ao formulário.

| Tipo de campo | Nome | Propósito | Oculto | Valor padrão |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | assinado | Para indicar o status da assinatura | Y | N |
| TextField | guid | Para identificar de forma exclusiva | Y | 3889 |
| TextField | customerName | Para capturar o nome do cliente | N |
| TextField | customerEmail | Email do cliente para enviar notificação | N |
| Caixa de seleção | formsToSign | Os itens identificam os formulários no pacote | N |

O formulário inicial precisa ser configurado para acionar um fluxo de trabalho de AEM chamado **formulários múltiplos**
Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga útil que processa o envio do formulário.

## Assets

O formulário inicial (Formulário de refinanciamento) pode ser [baixado aqui](assets/refinance-form.zip)

## Próximas etapas

[Criar formulários para assinatura](./create-forms-for-signing.md)
