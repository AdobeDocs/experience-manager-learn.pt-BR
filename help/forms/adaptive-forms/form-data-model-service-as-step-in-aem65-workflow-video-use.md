---
title: Utilização do serviço de modelo de dados de formulário como etapa no fluxo de trabalho do AEM 6.5
description: O AEM Forms 6.5 apresentou a capacidade de criar variáveis no workflow AEM. Com esse novo recurso, usar o "Serviço de modelo de dados de formulário de chamada" no fluxo de trabalho do AEM ficou muito fácil. O vídeo a seguir o guiará pelas etapas relativas ao uso do serviço Chamar modelo de dados de formulário no fluxo de trabalho do AEM.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 259
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# Utilização do serviço de modelo de dados de formulário como etapa no fluxo de trabalho do AEM 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora temos a capacidade de usar o Serviço de modelo de dados de formulário como parte do fluxo de trabalho do AEM. O vídeo a seguir aborda as etapas necessárias para configurar a etapa Modelo de dados de formulário no fluxo de trabalho do AEM

>O recurso demonstrado neste vídeo requer o AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

Para testar esse recurso no servidor, siga as instruções abaixo

* Configure o tomcat com o arquivo SampleRest.war conforme descrito [aqui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).O arquivo war implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato. A pontuação de crédito é um número aleatório entre 200 e 800

* [Importar os ativos para o AEM usando o gerenciador de pacotes](assets/aem65-loanapplication.zip)
* O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa do FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [FormuláriodeInscriçãoHipoteca](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, a variável [workflow de aplicativo de empréstimo](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/invokefdm651.PNG).
O fluxo de trabalho utiliza o componente Ou divisão para rotear o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, a aplicação será encaminhada para cavery.
