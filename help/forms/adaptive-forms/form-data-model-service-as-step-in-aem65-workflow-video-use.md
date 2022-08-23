---
title: Uso do serviço do Modelo de dados de formulário como etapa AEM fluxo de trabalho 6.5
description: O AEM Forms 6.5 apresentou a capacidade de criar variáveis no Fluxo de trabalho do AEM. Com esse novo recurso usando o "Invoke Form Data Model Service" em AEM fluxo de trabalho, ficou muito fácil. O vídeo a seguir guiará você pelas etapas envolvidas no uso do Invoke Form Data Model Service em AEM fluxo de trabalho.
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Uso do serviço do Modelo de dados de formulário como etapa AEM fluxo de trabalho 6.5 {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora temos a capacidade de usar o Serviço do Modelo de dados de formulário como parte AEM fluxo de trabalho. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário AEM fluxo de trabalho

>!![NOTE]O recurso demonstrado neste vídeo requer o AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

Para testar esse recurso em seu servidor, siga as instruções abaixo

* Configure o tomcat com o arquivo SampleRest.war conforme descrito [here](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).O arquivo war implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato.A pontuação de crédito é um número aleatório entre 200 e 800

* [ Importe os ativos no AEM usando o gerenciador de pacotes](assets/aem65-loanapplication.zip)
* O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [FormulárioDeAplicaçãoDeHipoteca](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, a variável [fluxo de trabalho loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/invokefdm651.PNG).
O workflow utiliza o componente Ou Dividir para direcionar o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, o aplicativo é roteado para cavery.
