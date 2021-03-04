---
title: Uso do Serviço do Modelo de Dados de Formulário como Etapa no Fluxo de Trabalho
seo-title: Uso do Serviço do Modelo de Dados de Formulário como Etapa no Fluxo de Trabalho
description: A partir do AEM Forms 6.4, agora temos a capacidade de usar o Form Data Model como parte do fluxo de trabalho do AEM. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM.
seo-description: A partir do AEM Forms 6.4, agora temos a capacidade de usar o Form Data Model como parte do fluxo de trabalho do AEM. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM.
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: Fluxo de trabalho
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# Uso do Serviço do Modelo de Dados de Formulário como Etapa no Fluxo de Trabalho {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora temos a capacidade de usar o Form Data Model como parte do fluxo de trabalho do AEM. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

Para testar esse recurso em seu servidor, siga as instruções abaixo
* [Baixe e implante o pacote](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue . Este é o pacote OSGI personalizado que define propriedades de metadados.
>!![NOTE]No AEM Forms 6.5 e superior, esse recurso está disponível para uso imediato, como  [descrito aqui](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configure tomcat com o arquivo SampleRest.war, conforme descrito [here](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html). O arquivo war implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato. A pontuação de crédito é um número aleatório entre 200 e 800

* [Importe os ativos no AEM usando o gerenciador de pacotes](assets/invoke-fdm-as-service-step.zip). O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [MortgaugeApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, o [workflow de aplicativo de empréstimo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/fdm-as-service-step-workflow.PNG).
O workflow utiliza o componente Ou Dividir para direcionar o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, o aplicativo é roteado para cavery
