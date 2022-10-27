---
title: Envio de emails no envio do formulário adaptável
seo-title: Sending Email on Adaptive Form Submission
description: Enviar email de confirmação no envio do formulário adaptável usando o componente enviar email
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# Envio de emails no envio do formulário adaptável {#sending-email-on-adaptive-form-submission}

Uma das ações comuns é enviar um email de confirmação para o remetente no envio bem-sucedido do formulário adaptativo. Para fazer isso, selecionaremos a ação &quot;Enviar email&quot; como envio.

Você pode usar o modelo de email ou apenas digitar o corpo do email, como mostrado nesta captura de tela abaixo.

Observe a sintaxe para inserir valores de campos de formulário no email. Também temos a opção de incluir anexos de formulário no email, marcando a caixa de seleção &quot;incluir anexos&quot; nas propriedades de configuração.

Quando o formulário adaptável for enviado, o recipient receberá um email.

![SendEmail](assets/sendemailaction.gif)

## Configurações necessárias {#configurations-needed}

Você terá que configurar o Serviço Day CQ Mail. Isso pode ser configurado, apontando o navegador para [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

A captura de tela mostra as propriedades de configuração do servidor do adobe mail.

![mailservice](assets/mailservice.png)

Para experimentar isso no servidor, siga estas instruções:

* [Importar os ativos](assets/timeoffrequest.zip) associado a este artigo no AEM usando o gerenciador de pacotes.

* Abra o [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Preencha os detalhes.Certifique-se de fornecer um endereço de email válido no campo de email.

* Enviar o formulário.
