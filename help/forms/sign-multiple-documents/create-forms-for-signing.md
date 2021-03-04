---
title: Criar formulários para assinatura
description: Crie formulários que precisam ser incluídos no pacote de assinatura.
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Desenvolvimento
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 1%

---


# Criar formulários para assinatura

A próxima etapa é criar os formulários adaptáveis que você deseja incluir no pacote. Lembre-se de aderir aos seguintes pontos ao criar formulários para assinatura:

* Certifique-se de que os formulários sejam baseados no modelo **SignMultipleForms**. Isso garante que os formulários sejam preenchidos previamente com os dados obtidos do banco de dados.

* Os formulários precisam ser configurados para usar o Adobe Sign e o campo signer1 precisa ser associado ao campo Email do cliente
* Os formulários também precisam ser associados ao clientLib chamado **getnextform**
* Os formulários precisam usar o componente Etapa de assinatura.
* O formulário também deve usar o componente personalizado **Assinar vários formulários**. Esse componente permite navegar até o próximo formulário para fazer logon no pacote.
* O envio do formulário deve ser configurado para acionar o fluxo de trabalho do AEM **Atualizar Status da Assinatura**
* Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura um arquivo chamado Data.xml na carga útil que processa o envio do formulário.

Depois de criar seu formulário, inclua o fragmento de formulário adaptável **common fields** no formulário. O fragmento será marcado como oculto. Esse fragmento contém os seguintes campos.

* **assinado**  - o campo para conter o status da assinatura
* **guid**  - Identificador exclusivo para identificar o formulário no pacote
* **customerEmail**  - Este campo contém o email do cliente



>[!NOTE]
>Se você deseja carregar dados de um formulário para outro no pacote, certifique-se de que os campos do formulário tenham um nome idêntico em todos os formulários.

## Todos os formulários concluídos

Depois que todos os formulários do pacote forem preenchidos e assinados, precisaremos exibir a mensagem apropriada. Essa mensagem é exibida com a ajuda do formulário Concluído. O formulário concluído é incluído nos formulários de amostra.

## Ativos

Os formulários de amostra, incluindo os usados neste tutorial, podem ser [baixados aqui](assets/forms-for-signing.zip)
