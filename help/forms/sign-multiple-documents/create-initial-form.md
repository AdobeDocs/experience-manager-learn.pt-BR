---
title: Criar o formulário inicial para acionar o processo
description: Crie o formulário inicial para acionar a notificação por email para iniciar o processo de assinatura.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 7%

---

# Criar formulário inicial

O formulário inicial (Formulário de refinanciamento) é usado para assinar vários formulários acionando o **Assinar vários Forms** Fluxo de trabalho do AEM. Você pode inserir valores de sua escolha, mas garantir que os seguintes campos sejam adicionados ao formulário.

| Tipo de campo | Nome | Propósito | Oculto | Valor padrão |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | assinado | Para indicar o status da assinatura | Y | N |
| TextField | guid | Para identificar exclusivamente o formulário | Y | 3889 |
| TextField | customerName | Para capturar o nome dos clientes | N |
| TextField | customerEmail | Email do cliente para enviar notificação | N |
| CheckBox | formsToSign | Os itens identificam os formulários no pacote | N |

O formulário inicial precisa ser configurado para acionar um workflow do AEM chamado **signmultipleforms**
Verifique se o Caminho do arquivo de dados está definido como **Dados.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga do processo para o envio do formulário.

## Ativos

O formulário inicial (Formulário de refinanciamento) pode ser [baixado aqui](assets/refinance-form.zip)

## Próximas etapas

[Criar formulários a serem usados para assinatura](./create-forms-for-signing.md)
