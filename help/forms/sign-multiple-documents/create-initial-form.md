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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# Criar formulário inicial

O formulário inicial (Formulário de refinanciamento) é usado para assinar vários formulários acionando o **Assinar vários Forms** Fluxo de trabalho do AEM. Você pode inserir valores de sua escolha, mas garantir que os seguintes campos sejam adicionados ao formulário.

| Tipo de campo | Nome | Propósito | Oculto | Valor padrão |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| CampoTexto | assinado | Para indicar o status da assinatura | Y | N |
| CampoTexto | guid | Para identificar exclusivamente o formulário | Y | 3889 |
| CampoTexto | customerName | Para capturar o nome dos clientes | N |
| CampoTexto | customerEmail | Email do cliente para enviar notificação | N |
| CheckBox | formsToSign | Os itens identificam os formulários no pacote | N |

O formulário inicial precisa ser configurado para acionar um workflow do AEM chamado **signmultipleforms**
Verifique se o Caminho do arquivo de dados está definido como **Dados.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga do processo para o envio do formulário.

## Assets

O formulário inicial (Formulário de refinanciamento) pode ser [baixado aqui](assets/refinance-form.zip)

## Próximas etapas

[Criar formulários a serem usados para assinatura](./create-forms-for-signing.md)
