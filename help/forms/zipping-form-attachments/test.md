---
title: Testar a solução
description: Teste a solução adicionando anexos ao formulário e acione o fluxo de trabalho para enviar o email.
sub-product: formulários
feature: Fluxo de trabalho
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: Desenvolvimento
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---


# Testar a solução


* Configure [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar emails do servidor AEM Forms
* Implante o pacote [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) usando [console Web felix](http://localhost:4502/system/console/bundles)
* Implante o workflow [SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Esse workflow usa o componente Enviar email para enviar o arquivo zipped_attachments.zip que é salvo na pasta payload pela etapa do processo personalizado. Configure os remetentes e os endereços de email dos recipients de acordo com suas necessidades.
* Importe o [formulário de amostra](assets/zip-form-attachments-form.zip) de [Forms e interface do usuário de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualize o ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) formulário, adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com o arquivo zip deve ser enviada.

