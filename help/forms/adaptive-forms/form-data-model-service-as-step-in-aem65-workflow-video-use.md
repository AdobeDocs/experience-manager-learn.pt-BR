---
title: Uso do serviço Form Data Model como Etapa do Fluxo de Trabalho AEM 6.5
seo-title: Uso do serviço Form Data Model como Etapa do Fluxo de Trabalho AEM 6.5
description: O AEM Forms 6.5 introduziu a capacidade de criar variáveis no Fluxo de trabalho do AEM. Com esse novo recurso usando o "Serviço Invocar Modelo de Dados de Formulário" em AEM Fluxo de trabalho, ficou muito fácil. O vídeo a seguir o guiará pelas etapas envolvidas no uso do serviço Invocar Modelo de Dados de Formulário no Fluxo de Trabalho AEM.
seo-description: O AEM Forms 6.5 introduziu a capacidade de criar variáveis no Fluxo de trabalho do AEM. Com esse novo recurso usando o "Serviço Invocar Modelo de Dados de Formulário" em AEM Fluxo de trabalho, ficou muito fácil. O vídeo a seguir o guiará pelas etapas envolvidas no uso do serviço Invocar Modelo de Dados de Formulário no Fluxo de Trabalho AEM.
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# Uso do serviço de modelo de dados de formulário como etapa no fluxo de trabalho AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora temos a capacidade de usar o serviço de Modelo de dados de formulário como parte do Fluxo de trabalho AEM. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário no AEM Fluxo de trabalho

>!![NOTE]O recurso demonstrado neste vídeo requer o AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Para testar esse recurso em seu servidor, siga as instruções abaixo

* Configure tomcat com o arquivo SampleRest.war conforme descrito [here](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).O arquivo de guerra implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato.A pontuação de crédito é um número aleatório entre 200 e 800

* [ Importar ativos para AEM usando o gerenciador de pacotes](assets/aem65-loanapplication.zip)
* O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, o fluxo de trabalho [loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/invokefdm651.PNG).
O fluxo de trabalho utiliza o componente Ou dividir para rotear o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, o aplicativo será direcionado para a recuperação.
