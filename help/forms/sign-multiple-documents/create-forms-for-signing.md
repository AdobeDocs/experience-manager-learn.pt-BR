---
title: Criar Forms para assinatura
description: Crie formulários que precisam ser incluídos no pacote de assinatura.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Criar formulários para assinatura

A próxima etapa é criar os formulários adaptáveis que você deseja incluir no pacote. Lembre-se de aderir aos seguintes pontos ao criar formulários para assinatura:

* Certifique-se de que os formulários sejam baseados no **SignMultipleForms** modelo . Isso garante que os formulários sejam preenchidos previamente com os dados obtidos do banco de dados.

* Os formulários precisam ser configurados para usar o Adobe Sign e o campo signer1 precisa ser associado ao campo Email do cliente
* Os formulários também precisam ser associados ao clientLib chamado **getnextform**
* Os formulários precisam usar o componente Etapa de assinatura.
* O formulário também deve usar o **Assinar vários formulários** componente. Esse componente permite navegar até o próximo formulário para fazer logon no pacote.
* O envio do formulário deve ser configurado para acionar AEM fluxo de trabalho **Atualizar Status da Assinatura**
* Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga útil que processa o envio do formulário.

Depois de criar seu formulário, inclua o **campos comuns** fragmento de formulário adaptável no formulário. O fragmento é marcado como oculto. Esse fragmento contém os seguintes campos.

* **assinado** - O campo para conter o status da assinatura
* **guid** - Identificador exclusivo para identificar o formulário no pacote
* **customerEmail** - Este campo contém o email do cliente



>[!NOTE]
>Se você deseja carregar dados de um formulário para outro no pacote, certifique-se de que os campos do formulário tenham um nome idêntico em todos os formulários.

## Todos os formulários concluídos

Depois que todos os formulários do pacote forem preenchidos e assinados, precisaremos exibir a mensagem apropriada. Essa mensagem é exibida com a ajuda do formulário Concluído. O formulário concluído é incluído nos formulários de amostra.

## Assets

Os formulários de amostra, incluindo os usados neste tutorial, podem ser [baixado aqui](assets/forms-for-signing.zip)
