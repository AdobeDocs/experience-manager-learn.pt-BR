---
title: Página Enviar Para Agradecimentos
seo-title: Submitting To Thank You Page
description: Exibir uma página de agradecimento ao enviar o formulário adaptável
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# Página Enviar Para Agradecimentos {#submitting-to-thank-you-page}

A opção Enviar para ponto de extremidade REST passa os dados preenchidos no formulário para uma página de confirmação configurada como parte da solicitação HTTP GET. É possível adicionar o nome dos campos a serem solicitados. O formato da solicitação é:

\{fieldName\} = \{parameterName\}. Por exemplo, submitterName é o nome de um campo de formulário adaptável e submitter é o nome do parâmetro. Na página de agradecimento, você pode acessar o parâmetro do remetente usando request.getParameter(&quot;submitter&quot;) para obter o valor do campo de nome do remetente.

`submitterName=submitter`

Na captura de tela abaixo, enviamos o formulário adaptável para agradecer a página localizada em /content/ankyou. Para essa página de agradecimento, estamos transmitindo 3 atributos de solicitação que contêm os valores de campo do formulário.

![Página de agradecimento](assets/thankyoupage.gif)

Também é possível enviar para o endpoint externo por meio do POST. Para isso, basta marcar a caixa de seleção &quot;ativar solicitação de postagem&quot; e fornecer o URL do endpoint externo. Ao enviar seu formulário, você recebe uma página de agradecimento e o ponto de extremidade POST é chamado simultaneamente.

![Configuração de captura](assets/capture.gif)

Para testar esse recurso em seu servidor, siga as instruções mencionadas abaixo:

* Importe o [arquivo de ativos associado a este artigo no AEM usando o gerenciador de pacotes](assets/submittingtorestendpoint.zip)
* Aponte seu navegador para o [Formulário de solicitação de tempo desligado](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha o campo obrigatório e envie o formulário
* Você deve obter a página de agradecimento com suas informações preenchidas na página
