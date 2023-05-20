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
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Criar o formulário adaptável principal

O formulário **ArmazenarAFWithAttachments** é o formulário adaptável principal. Este formulário adaptável é o ponto de entrada para o caso de uso. Nesse formulário, os detalhes do usuário, incluindo o número de celular, são capturados. Este formulário também pode adicionar alguns anexos. Quando o botão Salvar e sair é clicado, o código do lado do servidor é executado para armazenar os dados do formulário no banco de dados e uma ID exclusiva do aplicativo é gerada e apresentada ao usuário para segurança. Essa ID do aplicativo é usada para recuperar o número do celular associado ao aplicativo.

![formulário principal do aplicativo](assets/6552.JPG)

Este formulário está associado a **bootboxjs540,storeAFWithAttachments** bibliotecas de clientes criadas anteriormente no curso e um fluxo de trabalho AEM que é acionado no envio do formulário.


* Os formulários de amostra são baseados em [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisa ser importado para o AEM para que os formulários de amostra sejam renderizados corretamente.

* O concluído [Formulário StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) O pode ser baixado e importado para a instância do AEM.

* A variável [Fluxo de trabalho do AEM associado a este formulário](assets/workflow-model-store-af-with-attachments.zip) precisa ser importado para a instância do AEM para que o formulário funcione.


## Próximas etapas

[Criar o formulário recuperando o formulário salvo](./retrieve-saved-form.md)