---
title: Teste a solução - Etapas necessárias para testar as duas abordagens
description: Teste a solução adicionando anexos ao formulário e acione o fluxo de trabalho para enviar o email.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Etapas necessárias para testar as duas abordagens

* Configurar [Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) para enviar emails do servidor do AEM Forms
* Implante o [anexos de formulário](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) pacote usando [console da web felix](http://localhost:4502/system/console/bundles)

## Enviar arquivo zip como anexo de email



* Implante o [Fluxo de trabalho SendFormAttachmentsViaEmail .](assets/zipped-form-attachments-model.zip) Esse fluxo de trabalho usa o componente Enviar email para enviar o arquivo zipped_attachments.zip que é salvo na pasta payload pela etapa do processo personalizado. Configure os endereços de email dos remetentes e recipients de acordo com suas necessidades.
* Importe o [formulário de amostra](assets/zip-form-attachments-form.zip) from [Forms E Interface Do Usuário Do Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) e adicione alguns anexos e envie o formulário.
* O workflow deve ser acionado e uma notificação por email com o arquivo zip deve ser enviada.

## Enviar anexos de formulário como arquivos individuais

* Implante o [Fluxo de trabalho SendForm .](assets/send-form-attachments-model.zip) Esse workflow usa o componente Enviar email para enviar os anexos do formulário como arquivos individuais. Configure os remetentes e os endereços de email dos recipients de acordo com suas necessidades.
* Importe o [formulário de amostra](assets/send-list-attachments-form.zip) from [Forms E Interface Do Usuário Do Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) e adicione alguns anexos e envie o formulário.
* O fluxo de trabalho deve ser acionado e uma notificação por email com os anexos do formulário será enviada.
