---
title: Criar Forms para assinatura
description: Crie formulários que precisam ser incluídos no pacote de assinatura.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---


# Criar formulários para assinatura

A próxima etapa é criar os formulários adaptáveis que você deseja que sejam incluídos no pacote. Lembre-se de aderir aos seguintes pontos ao criar formulários para assinatura:

* Certifique-se de que os formulários sejam baseados no modelo **SignMultipleForms**. Isso garante que os formulários sejam pré-preenchidos com os dados obtidos do banco de dados.

* Os formulários precisam ser configurados para usar o Adobe Sign e o campo signer1 precisa ser associado ao campo Email do cliente
* Os formulários também precisam ser associados ao clientLib chamado **getnextform**
* Os formulários precisam usar o componente Etapa de assinatura.
* O formulário também deve usar o componente personalizado **Assinar vários formulários**. Esse componente permite que você navegue até o próximo formulário para fazer logon no pacote.
* O envio do formulário deve ser configurado para acionar AEM fluxo de trabalho **Atualizar Status da Assinatura**
* Verifique se o Caminho do arquivo de dados está definido como **Data.xml**. Isso é muito importante, pois o código de amostra procura por um arquivo chamado Data.xml na carga do processo de envio do formulário.

Depois de criar o formulário, inclua o fragmento de formulário adaptável **campos comuns** no formulário. O fragmento será marcado como oculto. Esse fragmento contém os seguintes campos.

* **assinado**  - O campo para manter o status da assinatura
* **guid**  - Identificador exclusivo para identificar o formulário no pacote
* **customerEmail**  - este campo contém o email do cliente



>[!NOTE]
>Se você quiser carregar dados de um formulário para outro formulário no pacote, certifique-se de que os campos do formulário tenham o mesmo nome em todos os formulários.

## Todos os formulários concluídos

Uma vez preenchidos e assinados todos os formulários do pacote, é necessário exibir a mensagem apropriada. Esta mensagem é exibida com a ajuda do formulário All-done. O formulário Acessado é incluído nos formulários de amostra.

## Ativos

Os formulários de amostra, incluindo os usados neste tutorial, podem ser [baixados daqui](assets/forms-for-signing.zip)
