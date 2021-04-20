---
title: Uso do serviço do Modelo de dados de formulário como etapa no fluxo de trabalho do AEM 6.5
seo-title: Uso do serviço do Modelo de dados de formulário como etapa no fluxo de trabalho do AEM 6.5
description: O AEM Forms 6.5 apresentou a capacidade de criar variáveis no fluxo de trabalho do AEM. Com esse novo recurso usando o "Invoke Form Data Model Service" no fluxo de trabalho do AEM tornou-se muito fácil. O vídeo a seguir guiará você pelas etapas envolvidas no uso do Invoke Form Data Model Service no fluxo de trabalho do AEM.
seo-description: O AEM Forms 6.5 apresentou a capacidade de criar variáveis no fluxo de trabalho do AEM. Com esse novo recurso usando o "Invoke Form Data Model Service" no fluxo de trabalho do AEM tornou-se muito fácil. O vídeo a seguir guiará você pelas etapas envolvidas no uso do Invoke Form Data Model Service no fluxo de trabalho do AEM.
feature: Workflow
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---


# Uso do serviço do Modelo de dados de formulário como etapa no fluxo de trabalho do AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora temos a capacidade de usar o Form Data Model Service como parte do fluxo de trabalho do AEM. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM

>!![NOTE]O recurso demonstrado neste vídeo requer o AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Para testar esse recurso em seu servidor, siga as instruções abaixo

* Configure tomcat com o arquivo SampleRest.war conforme descrito [here](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).O arquivo war implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato.A pontuação de crédito é um número aleatório entre 200 e 800

* [ Importe os ativos para o AEM usando o gerenciador de pacotes](assets/aem65-loanapplication.zip)
* O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [MortgaugeApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, o [workflow de aplicativo de empréstimo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/invokefdm651.PNG).
O workflow utiliza o componente Ou Dividir para direcionar o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, o aplicativo é roteado para cavery.
