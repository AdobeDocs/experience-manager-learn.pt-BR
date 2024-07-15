---
title: Criar Forms para assinatura
description: Crie formulários que precisam ser incluídos no pacote de assinatura.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 71
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Criar formulários para assinatura

A próxima etapa é criar os formulários adaptáveis que você deseja incluir no pacote. Lembre-se de seguir os seguintes pontos ao criar formulários para assinatura:

* Verifique se os formulários são baseados no modelo **SignMultipleForms**. Isso garante que os formulários sejam pré-preenchidos com os dados buscados no banco de dados.

* Os formulários precisam ser configurados para usar o Acrobat Sign e o campo signatário1 precisa ser associado ao campo Email do cliente
* Os formulários também precisam ser associados com clientLib chamado **getnextform**
* Os formulários precisam usar o componente Etapa de assinatura.
* O formulário também deve usar o componente personalizado **Assinar vários formulários**. Este componente permite navegar até o próximo formulário para entrar no pacote.
* O envio do formulário deve ser configurado para acionar o fluxo de trabalho do AEM **Atualizar status da assinatura**
* Verifique se o Caminho do Arquivo de Dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga do processo para o envio do formulário.

Depois de criar o formulário, inclua o fragmento de formulário adaptável **commonfields** no formulário. O fragmento está marcado como oculto. Este fragmento contém os seguintes campos.

* **assinado** - O campo que contém o status da assinatura
* **guid** - Identificador exclusivo para identificar o formulário no pacote
* **customerEmail** - Este campo contém o email do cliente



>[!NOTE]
>Se quiser carregar dados de um formulário para outro no pacote, verifique se os campos do formulário têm nomes idênticos em todos os formulários.

## Formulário Concluído

Depois que todos os formulários no pacote forem preenchidos e assinados, precisamos exibir a mensagem apropriada. Esta mensagem é exibida com a ajuda do formulário Alldone. O formulário Aldone é incluído nos formulários de amostra.

## Ativos

Os formulários de exemplo, incluindo o usado neste tutorial, podem ser [baixados daqui](assets/forms-for-signing.zip)

## Próximas etapas

[Testar a solução no sistema local](./testing-and-trouble-shooting.md)
