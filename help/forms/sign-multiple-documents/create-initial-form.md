---
title: Criar o formulário inicial para acionar o processo
description: Crie um formulário inicial para disparar a notificação por email para start do processo de assinatura.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 5%

---


# Criar formulário inicial

O formulário inicial (Formulário de refinanciamento) é usado para assinar vários formulários acionando o fluxo de trabalho de **Assinar vários Forms** AEM. Você pode inserir valores de sua escolha, mas garantir que os seguintes campos sejam adicionados ao formulário.



| Tipo de campo | Nome | Propósito | Oculto | Valor padrão |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | Para indicar o status da assinatura | S | N |
| TextField | guid | Para identificar de forma exclusiva o formulário | S | 3889 |
| TextField | customerName | Para capturar o nome do cliente | N |
| TextField | customerEmail | Email do cliente para enviar notificação | N |
| CheckBox | formsToSign | Os itens identificam os formulários no pacote | N |



O formulário inicial precisa ser configurado para acionar um fluxo de trabalho AEM chamado **signmultipleforms**
Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura por um arquivo chamado Data.xml na carga do processo de envio do formulário.

## Ativos

O formulário inicial (Formulário de Refinanciamento) pode ser [baixado daqui](assets/refinance-form.zip)





