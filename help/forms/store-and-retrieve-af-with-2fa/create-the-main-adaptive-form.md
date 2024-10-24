---
title: Criar o formulário adaptável principal
description: Crie os formulários adaptáveis para capturar as informações do candidato e o formulário adaptável para recuperar o formulário adaptável salvo
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Criar o formulário adaptável principal

O formulário **StoreAFWithAttachments** é o formulário adaptável principal. Este formulário adaptável é o ponto de entrada para o caso de uso. Nesse formulário, os detalhes do usuário, incluindo o número de celular, são capturados. Este formulário também pode adicionar alguns anexos. Quando o botão Salvar e sair é clicado, o código do lado do servidor é executado para armazenar os dados do formulário no banco de dados e uma ID exclusiva do aplicativo é gerada e apresentada ao usuário para segurança. Essa ID do aplicativo é usada para recuperar o número do celular associado ao aplicativo.

![formulário principal do aplicativo](assets/6552.JPG)

Este formulário está associado às bibliotecas de clientes **bootboxjs540,storeAFWithAttachments** criadas anteriormente no curso e a um fluxo de trabalho do AEM que é acionado no envio do formulário.


* Os formulários de amostra são baseados no [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisa ser importado para o AEM para que os formulários de amostra sejam renderizados corretamente.

* O [Formulário StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) concluído pode ser baixado e importado para a instância do AEM.

* O fluxo de trabalho do [AEM associado a este formulário](assets/workflow-model-store-af-with-attachments.zip) precisa ser importado para a instância do AEM para que o formulário funcione.


## Próximas etapas

[Criar o formulário recuperando o formulário salvo](./retrieve-saved-form.md)