---
title: Criar o formulário inicial para acionar o processo
description: Crie um formulário inicial para acionar a notificação por email para iniciar o processo de assinatura.
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: Desenvolvimento
role: Profissional
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 6%

---


# Criar formulário inicial

O formulário inicial (Formulário de refinanciamento) é usado para assinar vários formulários acionando o fluxo de trabalho **Assinar vários formulários** do AEM. Você pode inserir valores de sua escolha, mas garantir que os seguintes campos sejam adicionados ao formulário.



| Tipo de campo | Nome | Propósito | Oculto | Valor padrão |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | assinado | Para indicar o status da assinatura | S | N |
| TextField | guid | Para identificar de forma exclusiva | S | 3889 |
| TextField | customerName | Para capturar o nome do cliente | N |
| TextField | customerEmail | Email do cliente para enviar notificação | N |
| Caixa de seleção | formsToSign | Os itens identificam os formulários no pacote | N |



O formulário inicial precisa ser configurado para acionar um fluxo de trabalho do AEM chamado **signmultipleforms**
Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga útil que processa o envio do formulário.

## Ativos

O formulário inicial (Formulário de Refinanciamento) pode ser [baixado aqui](assets/refinance-form.zip)





