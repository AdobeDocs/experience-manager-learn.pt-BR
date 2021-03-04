---
title: Criar o formulário adaptável principal
description: Crie os formulários adaptáveis para capturar as informações do candidato e o formulário adaptável para recuperar o formulário adaptável salvo
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Desenvolvimento
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 1%

---


# Criar o formulário adaptável principal

O formulário **StoreAFWithAttachments** é o formulário adaptável principal. Esse formulário adaptável é o ponto de entrada para o caso de uso. Neste formulário, os detalhes do usuário, incluindo o número de celular, são capturados. Este formulário também tem a capacidade de adicionar alguns anexos. Quando o botão Salvar e Sair é clicado, o código do lado do servidor é executado para armazenar os dados do formulário no banco de dados, e uma ID de aplicativo exclusiva será gerada e apresentada ao usuário para manter a segurança. Essa id de aplicativo será usada para recuperar o número de celular associado ao aplicativo.

![formulário de candidatura principal](assets/6552.JPG)

Este formulário está associado às bibliotecas de clientes **bootboxjs540,storeAFWithAttachments** criadas anteriormente no curso e um fluxo de trabalho do AEM que é acionado no envio do formulário.


* Os formulários de amostra são baseados no [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisa ser importado para o AEM para que os formulários de amostra sejam renderizados corretamente.

* O [StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip) concluído pode ser baixado e importado para a instância do AEM.

* O [fluxo de trabalho do AEM associado a este formulário](assets/workflow-model-store-af-with-attachments.zip) precisa ser importado para a instância do AEM para que o formulário funcione.



