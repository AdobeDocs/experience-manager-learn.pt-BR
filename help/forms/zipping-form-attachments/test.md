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
duration: 53
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Etapas necessárias para testar as duas abordagens

* Configurar [Serviço de email Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar emails do servidor do AEM Forms
* Implante o [formattachments](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) pacote usando [felix web console](http://localhost:4502/system/console/bundles)

## Enviar arquivo zip como um anexo de email



* Implante o [Workflow SendFormAttachmentsViaEmail.](assets/zipped-form-attachments-model.zip) Esse fluxo de trabalho usa o componente de envio de email para enviar o arquivo zipped_attachments.zip, que é salvo na pasta de carga útil pela etapa de processo personalizada. Configure os endereços de email dos remetentes e destinatários de acordo com suas necessidades.
* Importe o [exemplo de formulário](assets/zip-form-attachments-form.zip) de [Forms e interface do usuário de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) e adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com o arquivo zip deve ser enviada.

## Enviar anexos de formulário como arquivos individuais

* Implante o [Fluxo de trabalho do SendForm.](assets/send-form-attachments-model.zip) Esse fluxo de trabalho usa o componente de envio de email para enviar os anexos de formulário como arquivos individuais. Configure o endereço de email dos remetentes e destinatários de acordo com suas necessidades.
* Importe o [exemplo de formulário](assets/send-list-attachments-form.zip) de [Forms e interface do usuário de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) e adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com os anexos de formulário é enviada.
