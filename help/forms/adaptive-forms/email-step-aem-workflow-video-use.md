---
title: Utilização da etapa Enviar email do Forms Workflow
description: A etapa Enviar email foi introduzida no AEM Forms 6.4. Com essa etapa, podemos criar processos ou fluxo de trabalho comerciais que permitirão enviar emails com ou sem anexos. O vídeo a seguir apresenta as etapas para configurar o componente Enviar email
feature: Fluxo de trabalho
version: 6.4,6.5
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---


# Utilização da etapa Enviar email do Forms Workflow {#using-send-email-step-of-forms-workflow}

A etapa Enviar email foi introduzida no AEM Forms 6.4. Com essa etapa, podemos criar processos ou fluxo de trabalho comerciais que permitirão enviar emails com ou sem anexos. O vídeo a seguir apresenta as etapas para configurar o componente Enviar email .

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Como parte deste artigo, abordaremos o seguinte caso de uso:

1. Um usuário preenche o Formulário de solicitação de tempo de desativação
1. No envio do formulário, AEM fluxo de trabalho é acionado
1. O Fluxo de trabalho AEM usa o componente Enviar email para enviar um email com o DoR como anexo

Antes de usar a etapa Enviar email , certifique-se de configurar o Day CQ Mail Service a partir do [configMgr](http://localhost:4502/system/console/configMgr). Fornecer os valores específicos do seu ambiente

![Configurar o Day CQ Mail Service](assets/mailservice.png)

Como parte dos ativos associados a este artigo, você obterá o seguinte

1. Formulário adaptável que acionará o fluxo de trabalho no envio
1. Exemplo de fluxo de trabalho que enviará um email com DOR como anexo
1. Pacote OSGi que cria as propriedades de metadados

Para executar a amostra em seu sistema, faça o seguinte:

1. [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Baixar e instalar ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)pacote setvalueEste pacote contém o código para criar as propriedades de metadados como parte da etapa do processo do fluxo de trabalho.
1. [Configurar o Day CQ Mail Service](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importe e instale os ativos associados a este artigo usando o gerenciador de pacotes no CRX](assets/emaildoraemformskt.zip)
1. Inicie o [formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Preencha os campos obrigatórios e envie.
1. Você deve receber um email com DocumentOfRecord como anexo

Explore o [modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Dê uma olhada na etapa do processo do fluxo de trabalho. O código personalizado associado à etapa do processo criará nomes de propriedades de metadados e definirá seus valores a partir dos dados enviados. Esses valores são então usados pelo componente de email de envio.

>[!NOTE]
>
>No AEM Forms 6.5 e superior, não é necessário esse código personalizado para criar propriedades de metadados. Use o recurso de variáveis AEM fluxo de trabalho

Verifique se a guia Anexos do componente Enviar email está configurada de acordo com a captura de tela abaixo
![Send Email Attachment Tab](assets/sendemailcomponentconfigure.jpg)O valor &quot;DOR.pdf&quot; tem de corresponder ao valor especificado no Document of Record Path especificado nas opções de submissão do formulário adaptável.

