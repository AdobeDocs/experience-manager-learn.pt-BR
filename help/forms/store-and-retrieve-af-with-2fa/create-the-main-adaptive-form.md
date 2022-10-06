---
title: Criar o formulário adaptável principal
description: Crie os formulários adaptáveis para capturar as informações do candidato e o formulário adaptável para recuperar o formulário adaptável salvo
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Criar o formulário adaptável principal

O formulário **StoreAFWithAttachments** é o formulário adaptável principal. Esse formulário adaptável é o ponto de entrada para o caso de uso. Neste formulário, os detalhes do usuário, incluindo o número de celular, são capturados. Este formulário também tem a capacidade de adicionar alguns anexos. Quando o botão Salvar e Sair é clicado, o código do lado do servidor é executado para armazenar os dados do formulário no banco de dados e uma ID de aplicativo exclusiva é gerada e apresentada ao usuário para manter a segurança. Esta id de aplicativo é usada para recuperar o número de celular associado ao aplicativo.

![formulário de candidatura principal](assets/6552.JPG)

Este formulário está associado a **bootboxjs540,storeAFWithAttachments** bibliotecas de clientes criadas anteriormente no curso e um fluxo de trabalho de AEM que é acionado no envio do formulário.


* Os formulários de amostra são baseados em [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisam ser importadas para AEM para que os formulários de amostra sejam renderizados corretamente.

* A [Formulário StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) O pode ser baixado e importado para a instância do AEM.

* O [Fluxo de trabalho AEM associado a este formulário](assets/workflow-model-store-af-with-attachments.zip) precisa ser importado para a instância de AEM para que o formulário funcione.
