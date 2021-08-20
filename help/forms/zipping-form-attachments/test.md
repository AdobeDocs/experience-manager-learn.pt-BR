---
title: Testar a solução
description: Teste a solução adicionando anexos ao formulário e acione o fluxo de trabalho para enviar o email.
feature: Formulários adaptáveis
version: 6.5
topic: Desenvolvimento
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Etapas necessárias para testar as duas abordagens

* Configure [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar emails do servidor AEM Forms
* Implante o pacote [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) usando [console Web felix](http://localhost:4502/system/console/bundles)

## Enviar arquivo zip como anexo de email



* Implante o workflow [SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Esse workflow usa o componente de email de envio para enviar o arquivo zipped_attachments.zip que é salvo na pasta payload pela etapa do processo personalizado. Configure os endereços de email dos remetentes e recipients de acordo com suas necessidades.
* Importe o [formulário de amostra](assets/zip-form-attachments-form.zip) de [Forms e interface do usuário de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualize o ](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) formulário, adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com o arquivo zip deve ser enviada.

## Enviar anexos de formulário como arquivos individuais

* Implante o workflow [SendForm .](assets/send-form-attachments-model.zip) Esse workflow usa o componente Enviar email para enviar os anexos do formulário como arquivos individuais. Configure os remetentes e os endereços de email dos recipients de acordo com suas necessidades.
* Importe o [formulário de amostra](assets/send-list-attachments-form.zip) de [Forms e interface do usuário de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualize o ](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) formulário, adicione alguns anexos e envie o formulário.
* O fluxo de trabalho deve ser acionado e uma notificação por email com os anexos do formulário será enviada.
