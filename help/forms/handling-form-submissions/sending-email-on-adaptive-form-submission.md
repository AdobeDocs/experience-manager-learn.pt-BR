---
title: Envio de email no envio do formulário adaptável
seo-title: Sending Email on Adaptive Form Submission
description: Enviar email de confirmação no envio do formulário adaptável usando o componente de envio de email
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

# Envio de email no envio do formulário adaptável {#sending-email-on-adaptive-form-submission}

Uma das ações comuns é enviar um email de confirmação para o remetente no envio bem-sucedido do Formulário adaptável. Para fazer isso, selecionaremos a ação &quot;Enviar email&quot; como envio.

Você pode usar o template de email ou apenas digitar o corpo do email, conforme mostrado nesta captura de tela abaixo.

Observe a sintaxe para inserir valores de campos de formulário no email. Também temos a opção de incluir anexos de formulário no email, marcando a caixa de seleção &quot;incluir anexos&quot; nas propriedades de configuração.

Quando o Formulário adaptável for enviado, o destinatário receberá um email.

![SendEmail](assets/sendemailaction.gif)

## Configurações necessárias {#configurations-needed}

Você terá que configurar o serviço Day CQ Mail. Isso pode ser configurado apontando seu navegador para [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

A captura de tela mostra as propriedades de configuração do servidor de email da Adobe.

![mailservice](assets/mailservice.png)

Para tentar isso no servidor, siga estas instruções:

* [Importar os ativos](assets/timeoffrequest.zip) associado a este artigo no AEM usando o gerenciador de pacotes.

* Abra o [FormuláriodeSolicitaçãodeFolga](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Preencha os detalhes. Forneça um endereço de email válido no campo de email.

* Enviar o formulário.
