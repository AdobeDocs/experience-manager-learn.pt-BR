---
title: Preencher tabela de formulários adaptáveis
description: Preencher a tabela de formulários adaptáveis com os resultados das chamadas do serviço de modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 57
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Preencher tabela de formulários adaptáveis com os resultados da chamada do serviço de modelo de dados de formulário

[O formulário disponível está hospedado aqui](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Neste artigo, observamos o preenchimento da tabela de formulários adaptáveis, buscando dados da invocação do serviço do modelo de dados de formulário. Vamos criar um cronograma de amortização em uma tabela que lista cada pagamento regular em uma hipoteca ao longo do tempo. Os resultados da amortização são retornados pelo Modelo de dados do formulário. O serviço do Modelo de dados de formulário é chamado no evento de clique do botão calcular, conforme mostrado na captura de tela. Os parâmetros de entrada e saída da chamada de serviço são mapeados adequadamente, conforme mostrado na captura de tela. A saída é mapeada para as colunas da Linha 1
![clickevent](assets/amortization.PNG)

A linha 1 é configurada para ser expandida dependendo dos dados retornados pela chamada de serviço. Observe as configurações de repetição especificadas aqui. Um valor -1 indica um número ilimitado de linhas na tabela
![Linha1](assets/rowconfiguration.PNG)

## Implantar no servidor

[Instale o Tomcat conforme especificado aqui](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Implantar o arquivo SampleRest.war contido neste arquivo zip no Tomcat](assets/sample-rest.zip)
[Instalar os ativos](assets/amortizationschedule.zip) uso do gerenciador de pacotes AEM
[Abra o Formulário de programação de amortização](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Insira o valor apropriado e clique em Calcular programação de amortização deve ser preenchida no formulário
