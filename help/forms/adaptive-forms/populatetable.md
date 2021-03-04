---
title: 'Preencher tabela de formulário adaptável '
seo-title: Preencher tabela de formulário adaptável
description: Preencha a tabela de formulário adaptável com os resultados das chamadas do serviço de modelo de dados de formulário
seo-description: Preencha a tabela de formulário adaptável com os resultados das chamadas do serviço de modelo de dados de formulário
feature: formulários adaptáveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---


# Preencha a tabela de formulário adaptável com os resultados da chamada do serviço de modelo de dados de formulário

[O formulário em tempo real está hospedado ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
aqui. Neste artigo, obtemos o preenchimento da tabela de formulários adaptáveis buscando dados a partir da invocação do serviço de modelo de dados de formulário. Vamos criar um agendamento de amortização em uma tabela que lista cada pagamento regular em uma hipoteca ao longo do tempo. Os resultados da amortização são retornados pelo nosso Modelo de dados de formulário. O serviço do Modelo de dados de formulário é chamado no evento click do botão calcular, como mostrado na captura de tela. Os parâmetros de entrada e de saída da chamada de serviço são mapeados adequadamente, conforme mostrado na captura de tela. A saída é mapeada para as colunas de Row1
![clickevent](assets/amortization.PNG)

A Linha1 está configurada para crescer, dependendo dos dados retornados pela chamada de serviço. Observe as configurações de repetição especificadas aqui. Um valor de -1 indica um número ilimitado de linhas na tabela
![Linha1](assets/rowconfiguration.PNG)

## Implante no servidor

[Instale o Tomcat conforme especificado ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[aquiImplante o ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[arquivo SampleRest.warInstale os ativos  ](assets/amortizationschedule.zip) usando o gerenciador de pacotes AEM 
[Abra o ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Formulário de agendamento de personalizaçãoInsira o valor apropriado e clique em calcular Agendamento de Amortização deve ser preenchido em seu formulário

