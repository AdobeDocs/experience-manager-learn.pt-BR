---
title: Testar a solução - Etapas necessárias para testar as duas abordagens
description: Teste a solução adicionando anexos ao formulário e acione o fluxo de trabalho para enviar o email.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Etapas necessárias para testar as duas abordagens

* Configure o [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar emails do servidor AEM Forms
* Implante o pacote [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) usando o [felix web console](http://localhost:4502/system/console/bundles)

## Enviar arquivo zip como um anexo de email



* Implante o fluxo de trabalho [SendFormAttachmentsViaEmail.](assets/zipped-form-attachments-model.zip) Este fluxo de trabalho usa o componente de envio de email para enviar o arquivo zipped_attachments.zip, que é salvo na pasta de carga pela etapa de processo personalizada. Configure os endereços de email dos remetentes e destinatários de acordo com suas necessidades.
* Importar o [formulário de exemplo](assets/zip-form-attachments-form.zip) da [Forms e da Interface do Usuário do Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualize o formulário](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled), adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com o arquivo zip deve ser enviada.

## Enviar anexos de formulário como arquivos individuais

* Implante o fluxo de trabalho [SendForm.](assets/send-form-attachments-model.zip) Este fluxo de trabalho usa o componente de envio de email para enviar os anexos de formulário como arquivos individuais. Configure o endereço de email dos remetentes e destinatários de acordo com suas necessidades.
* Importar o [formulário de exemplo](assets/send-list-attachments-form.zip) da [Forms e da Interface do Usuário do Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualize o formulário](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled), adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com os anexos de formulário é enviada.
