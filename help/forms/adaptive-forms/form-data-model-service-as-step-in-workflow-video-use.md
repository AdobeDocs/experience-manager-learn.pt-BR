---
title: Uso do Serviço do Modelo de Dados de Formulário como Etapa no Fluxo de Trabalho
description: A partir do AEM Forms 6.4, agora temos a capacidade de usar o Modelo de dados de formulário como parte AEM fluxo de trabalho. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário AEM fluxo de trabalho.
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Uso do Serviço do Modelo de Dados de Formulário como Etapa no Fluxo de Trabalho {#using-form-data-model-service-as-step-in-workflow}

A partir do AEM Forms 6.4, agora temos a capacidade de usar o Modelo de dados de formulário como parte AEM fluxo de trabalho. O vídeo a seguir apresenta as etapas necessárias para configurar a etapa Modelo de dados de formulário AEM fluxo de trabalho


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

Para testar esse recurso em seu servidor, siga as instruções abaixo
* [Baixe e implante o pacote setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este é o pacote OSGI personalizado que define propriedades de metadados.
>!![NOTE]No AEM Forms 6.5 e superior, esse recurso está disponível para uso imediato como [descreva aqui](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* Configure o tomcat com o arquivo SampleRest.war conforme descrito [here](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).O arquivo war implantado no Tomcat tem o código para retornar a pontuação de crédito do candidato. A pontuação de crédito é um número aleatório entre 200 e 800

* [Importe os ativos no AEM usando o gerenciador de pacotes](assets/invoke-fdm-as-service-step.zip).O pacote contém o seguinte:

   * Modelo de fluxo de trabalho que usa a etapa FDM.
   * Modelo de dados de formulário usado na etapa FDM.
   * Formulário adaptável para acionar o fluxo de trabalho no envio.
* Abra o [FormulárioDeAplicaçãoDeHipoteca](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). Preencha os detalhes e envie. No envio do formulário, a variável [fluxo de trabalho loanapplication](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) é acionado.

![ fluxo de trabalho ](assets/fdm-as-service-step-workflow.PNG).
O workflow utiliza o componente Ou Dividir para direcionar o aplicativo para o administrador se a pontuação de crédito for superior a 500. Se a pontuação de crédito for inferior a 500, o aplicativo é roteado para cavery
