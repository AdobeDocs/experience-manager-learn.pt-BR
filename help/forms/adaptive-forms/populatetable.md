---
title: Preencher tabela de formulário adaptável
description: Preencha a tabela de formulário adaptável com os resultados das chamadas do serviço de modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Preencha a tabela de formulário adaptável com os resultados da chamada do serviço de modelo de dados de formulário

[O formulário ao vivo está hospedado aqui](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Neste artigo, observamos o preenchimento da tabela de formulários adaptáveis buscando dados a partir da invocação do serviço de modelo de dados de formulário. Vamos criar um agendamento de amortização em uma tabela que lista cada pagamento regular em uma hipoteca ao longo do tempo. Os resultados da amortização são retornados pelo nosso Modelo de dados de formulário. O serviço do Modelo de dados de formulário é chamado no evento click do botão calcular, como mostrado na captura de tela. Os parâmetros de entrada e de saída da chamada de serviço são mapeados adequadamente, conforme mostrado na captura de tela. A saída é mapeada para as colunas de Row1
![clickevent](assets/amortization.PNG)

A Linha1 está configurada para crescer, dependendo dos dados retornados pela chamada de serviço. Observe as configurações de repetição especificadas aqui. Um valor de -1 indica um número ilimitado de linhas na tabela
![Linha1](assets/rowconfiguration.PNG)

## Implante no servidor

[Instale o Tomcat conforme especificado aqui](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Implante o arquivo SampleRest.war contido neste arquivo zip em seu Tomcat](assets/sample-rest.zip)
[Instalar os ativos ](assets/amortizationschedule.zip) usando o gerenciador de pacotes AEM
[Abrir o Formulário de Programação de Amortização](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Insira o valor apropriado e clique em calcular Agendamento de Amortização deve ser preenchido em seu formulário
