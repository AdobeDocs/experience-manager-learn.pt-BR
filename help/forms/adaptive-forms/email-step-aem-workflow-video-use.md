---
title: Etapa de envio de e-mail do Forms Workflow
description: A etapa Enviar email foi introduzida no AEM Forms 6.4. Usando esta etapa, podemos criar processos de negócios ou fluxo de trabalho que permitirão enviar emails com ou sem anexos. O vídeo a seguir mostra as etapas para configurar o componente de envio de email
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 0%

---

# Etapa de envio de e-mail do Forms Workflow {#using-send-email-step-of-forms-workflow}

A etapa Enviar email foi introduzida no AEM Forms 6.4. Usando esta etapa, podemos criar processos de negócios ou fluxo de trabalho que permitirão enviar emails com ou sem anexos. O vídeo a seguir mostra as etapas para configurar o componente de envio de email.

>[!VIDEO](https://video.tv.adobe.com/v/34847?quality=12&learn=on&captions=por_br)

Como parte deste artigo, guiaremos você pelo seguinte caso de uso:

1. Um usuário preenche o Formulário de solicitação de folga
1. No envio do formulário, o fluxo de trabalho do AEM é acionado
1. O fluxo de trabalho do AEM usa o componente Enviar email para enviar um email com o DoR como um anexo

Antes de usar a etapa Enviar Email, configure o Serviço de Email Day CQ do [configMgr](http://localhost:4502/system/console/configMgr). Forneça os valores específicos do seu ambiente

![Configurar o Day CQ Mail Service](assets/mailservice.png)

Como parte dos ativos associados a este artigo, você obterá o seguinte

1. Formulário adaptável que acionará o fluxo de trabalho no envio
1. Exemplo de fluxo de trabalho que enviará um email com DOR como anexo
1. Pacote OSGi que cria as propriedades de metadados

Para executar o exemplo em seu sistema, faça o seguinte:

1. [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Baixar e instalar o conjunto setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)Este conjunto contém o código para criar as propriedades de metadados como parte da etapa de processo do fluxo de trabalho.
1. [Configurar o Day CQ Mail Service](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importe e instale os ativos associados a este artigo usando o gerenciador de pacotes no CRX](assets/emaildoraemformskt.zip)
1. Inicie o [formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Preencha os campos obrigatórios e envie.
1. Você deve receber um email com DocumentOfRecord como anexo

Explore o [modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Consulte a etapa do processo do fluxo de trabalho. O código personalizado associado à etapa do processo criará nomes de propriedades de metadados e definirá seus valores a partir dos dados enviados. Esses valores são usados pelo componente de envio de email.

>[!NOTE]
>
>No AEM Forms 6.5 e superior, você não precisa desse código personalizado para criar propriedades de metadados. Use o recurso de variáveis no AEM Workflow

Verifique se a guia Anexos do componente Enviar email está configurada de acordo com a captura de tela abaixo
![Guia Enviar Anexo de Email](assets/sendemailcomponentconfigure.jpg)O valor &quot;DOR.pdf&quot; deve corresponder ao valor especificado no Caminho do Documento de Registro especificado nas opções de envio do formulário adaptável.
