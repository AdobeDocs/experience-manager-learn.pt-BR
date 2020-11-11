---
title: Criar o formulário adaptativo principal
description: Crie os formulários adaptáveis para capturar as informações do candidato e o formulário adaptável para recuperar o formulário adaptativo salvo
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# Criar o formulário adaptativo principal

O formulário **StoreAFWithAttachments** é o formulário adaptativo principal. Este formulário adaptável é o ponto de entrada para o caso de uso. Neste formulário, os detalhes do usuário, incluindo o número do celular, são capturados. Esse formulário também tem a capacidade de adicionar alguns anexos. Quando o botão Salvar e Sair é clicado, o código do lado do servidor é executado para armazenar os dados do formulário no banco de dados e uma ID exclusiva do aplicativo será gerada e apresentada ao usuário para manter a segurança. Essa ID do aplicativo será usada para recuperar o número do dispositivo móvel associado ao aplicativo.

![formulário principal de candidatura](assets/6552.JPG)

Este formulário está associado às bibliotecas clientes **bootboxjs540,storeAFWithAttachments** criadas anteriormente no curso e a um fluxo de trabalho AEM que é acionado no envio do formulário.


* Os formulários de amostra são baseados no modelo [de formulário adaptável](assets/custom-template-with-page-component.zip) personalizado que precisa ser importado para AEM para que os formulários de amostra sejam renderizados corretamente.

* O formulário [](assets/store-af-with-attachments-form.zip) StoreAfWithAttachments concluído pode ser baixado e importado para a instância AEM.

* O fluxo de trabalho [AEM associado a este formulário](assets/workflow-model-store-af-with-attachments.zip) precisa ser importado para a instância AEM para que o formulário funcione.



