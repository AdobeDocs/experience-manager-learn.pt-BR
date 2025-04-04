---
title: Enviando Para A Página De Agradecimento
description: Exibir uma página de agradecimento ao enviar o Formulário adaptável
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 51
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# Enviando Para A Página De Agradecimento {#submitting-to-thank-you-page}

A opção Enviar para endpoint REST passa os dados preenchidos no formulário para uma página de confirmação configurada como parte da solicitação HTTP GET. Você pode adicionar o nome dos campos a serem solicitados. O formato da solicitação é:

\{fieldName\} = \{parameterName\}. Por exemplo, submitterName é o nome de um campo de formulário adaptável e submitter é o nome do parâmetro. Na página de agradecimento, você pode acessar o parâmetro do emissor usando request.getParameter(&quot;emissor&quot;) para obter o valor do campo de nome do emissor.

`submitterName=submitter`

Na captura de tela abaixo, estamos enviando o formulário adaptável para a página de agradecimento localizada em /content/thank you. Para esta página de agradecimento, estamos passando 3 atributos de solicitação que contêm os valores do campo de formulário.

![Página de agradecimento](assets/thankyoupage.gif)

Também é possível enviar para o endpoint externo via POST. Para fazer isso, basta marcar a caixa de seleção &quot;habilitar solicitação post&quot; e fornecer o URL para o endpoint externo. Ao enviar seu formulário, você recebe uma página de agradecimento e o terminal POST é chamado simultaneamente.

![Capturar configuração](assets/capture.gif)

Para testar esse recurso no servidor, siga as instruções mencionadas abaixo:

* Importar o [arquivo de ativos associado a este artigo para o AEM usando o gerenciador de pacotes](assets/submittingtorestendpoint.zip)
* Aponte seu navegador para o [Formulário de solicitação de folga](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha o campo obrigatório e envie o formulário
* Você deve receber a página de agradecimento com suas informações preenchidas na página
