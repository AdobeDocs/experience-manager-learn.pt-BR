---
title: Criar o formulário inicial para acionar o processo
description: Crie um formulário inicial para acionar a notificação por email para iniciar o processo de assinatura.
feature: Formulários adaptáveis
version: 6.4,6.5
topic: Desenvolvimento
role: Business Practitioner
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 7%

---


# Criar formulário inicial

O formulário inicial (Formulário de refinanciamento) é usado para assinar vários formulários acionando o fluxo de trabalho de AEM **Assinar vários Forms**. Você pode inserir valores de sua escolha, mas garantir que os seguintes campos sejam adicionados ao formulário.

| Tipo de campo | Nome | Propósito | Oculto | Valor padrão |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | assinado | Para indicar o status da assinatura | S | N |
| TextField | guid | Para identificar de forma exclusiva | S | 3889 |
| TextField | customerName | Para capturar o nome do cliente | N |
| TextField | customerEmail | Email do cliente para enviar notificação | N |
| Caixa de seleção | formsToSign | Os itens identificam os formulários no pacote | N |

O formulário inicial precisa ser configurado para acionar um fluxo de trabalho de AEM chamado **signmultipleforms**
Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga útil que processa o envio do formulário.

## Ativos

O formulário inicial (Formulário de Refinanciamento) pode ser [baixado aqui](assets/refinance-form.zip)





