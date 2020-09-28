---
title: Envio de email no envio de formulário adaptável
seo-title: Envio de email no envio de formulário adaptável
description: Enviar email de confirmação no envio de formulário adaptável usando o componente de email de envio
seo-description: Enviar email de confirmação no envio de formulário adaptável usando o componente de email de envio
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: adaptive-forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
translation-type: tm+mt
source-git-commit: 272e43ee4aeb756a3a1fd0ce55eaca94a9553fa4
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Envio de email no envio de formulário adaptável {#sending-email-on-adaptive-form-submission}

Uma das ações comuns é enviar um email de confirmação para o remetente após o envio bem-sucedido do Formulário adaptativo. Para conseguir isso, selecionaremos &quot;Enviar email&quot; como ação de envio.

Você pode usar o modelo de e-mail ou apenas digitar o corpo do e-mail, como mostrado nesta captura de tela abaixo.

Observe a sintaxe para inserir valores de campos de formulário no email.Também temos a opção de incluir anexos de formulário no email, marcando a caixa de seleção &quot;incluir anexos&quot; nas propriedades de configuração.

Quando o Formulário adaptativo for enviado, o recipient receberá um email.

![SendEmail](assets/sendemailaction.gif)

## Configurações necessárias {#configurations-needed}

Você terá que configurar o serviço Day CQ Mail. Isso pode ser configurado apontando seu navegador para o [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

A captura de tela mostra as propriedades de configuração do adobe mail server.

![serviço postal](assets/mailservice.png)

Para experimentar isso no servidor, siga estas instruções:

* [Importe os ativos](assets/timeoffrequest.zip) associados a este artigo em AEM usando o gerenciador de pacotes.

* Abra o [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Preencha os detalhes.Certifique-se de fornecer um endereço de email válido no campo de email.

* Envie o formulário.
