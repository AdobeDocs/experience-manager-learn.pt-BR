---
title: Usar o serviço de modelo de dados de formulário como etapa no fluxo de trabalho
description: A partir do AEM Forms 6.4, agora podemos usar o Modelo de dados de formulário como parte do fluxo de trabalho do AEM. O vídeo a seguir aborda as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 205
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Usar o serviço de modelo de dados de formulário como etapa no fluxo de trabalho {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora podemos usar o Modelo de dados de formulário como parte do fluxo de trabalho do AEM. O vídeo a seguir aborda as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Para testar esse recurso no servidor, siga as instruções abaixo
* [Baixe e implante o conjunto setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este é o pacote OSGI personalizado que define as propriedades dos metadados.
>No AEM Forms 6.5 e posterior, esse recurso está disponível imediatamente, conforme [descrito aqui](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configure o tomcat com o arquivo SampleRest.war como descrito [aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html). O arquivo war implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato. A pontuação de crédito é um número aleatório entre 200 e 800

* [Importe os ativos para o AEM usando o gerenciador de pacotes](assets/invoke-fdm-as-service-step.zip). O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa do FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, o [fluxo de trabalho do aplicativo de empréstimo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/fdm-as-service-step-workflow.PNG).
O fluxo de trabalho utiliza o componente Ou divisão para rotear o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, a aplicação será encaminhada para a cavery
