---
title: Página Enviar Para Agradecimentos
seo-title: Página Enviar Para Agradecimentos
description: Exibir uma página de agradecimento ao enviar o formulário adaptável
seo-description: Exibir uma página de agradecimento ao enviar o formulário adaptável
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
translation-type: tm+mt
source-git-commit: 0bccdea82f6db391cbca3ab06009a7b4420a38bf
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Enviando para a página de agradecimento {#submitting-to-thank-you-page}

A opção Enviar para ponto de extremidade REST transmite os dados preenchidos no formulário para uma página de confirmação configurada como parte da solicitação de GET HTTP. É possível adicionar o nome dos campos a serem solicitados. O formato da solicitação é:

\{fieldName\} = \{parameterName\}. Por exemplo, o submitterName é o nome de um campo de formulário adaptável e o remetente é o nome do parâmetro. Na página de agradecimento, você pode acessar o parâmetro do remetente usando request.getParameter(&quot;submitter&quot;) para obter a retenção do valor do campo de nome do remetente.

submitterName=submitter

Na captura de tela abaixo, enviamos o formulário adaptável para agradecer a sua página localizada em /content/ankyou. Para esta página de agradecimento, estamos transmitindo 3 atributos de solicitação que manterão os valores do campo de formulário.

![agradeço](assets/thankyoupage.gif)

Também é possível enviar para o terminal externo por POST. Para fazer isso, basta selecionar a caixa de seleção &quot;ativar solicitação de postagem&quot; e fornecer o URL do terminal externo. Ao enviar seu formulário, você receberá a página de agradecimento e o terminal POST será chamado simultaneamente.

![captura](assets/capture.gif)


Para testar esse recurso em seu servidor, siga as instruções mencionadas abaixo:

* Importe o arquivo [assets associado a este artigo para AEM usando o gerenciador de pacote](assets/submittingtorestendpoint.zip)
* Aponte seu navegador para o [Formulário de solicitação de tempo limite](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Preencha o campo obrigatório e envie o formulário
* Você deve obter a página de agradecimento com suas informações preenchidas na página

