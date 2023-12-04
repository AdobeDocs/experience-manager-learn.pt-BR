---
title: Variáveis no fluxo de trabalho do AEM[Part4]
description: Uso de variáveis do tipo XML, JSON, ArrayList, Document em um workflow AEM
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: 269e43f7-24cf-4786-9439-f51bfe91d39c
duration: 139
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Variável ArrayList no fluxo de trabalho do AEM

Variáveis do tipo ArrayList foram introduzidas no AEM Forms 6.5. Um caso de uso comum para o uso da variável ArrayList é definir rotas personalizadas a serem usadas em AssignTask.

Para usar a variável ArrayList em um workflow para AEM, é necessário criar um Formulário adaptável que gere elementos repetidos nos dados enviados. Uma prática comum é definir um schema que contém um elemento de matriz. Para os fins deste artigo, criei um esquema JSON simples contendo elementos de matriz. O caso de uso é de um funcionário preenchendo um relatório de despesas. No relatório de despesas, capturamos o nome do gerente do remetente e o nome do gerente. Os nomes do gerenciador são armazenados em uma matriz chamada managerchain. A captura de tela abaixo mostra o formulário do relatório de despesas e os dados do envio do Adaptive Forms.

![relatório de despesas](assets/expensereport.jpg)

A seguir estão os dados do envio do formulário adaptável. O formulário adaptável foi baseado no schema JSON. Os dados vinculados ao esquema são armazenados no elemento de dados do elemento afBoundData. A cadeia de gerenciamento é uma matriz e precisamos preencher a ArrayList com o elemento name do objeto dentro da matriz da cadeia de gerenciamento.

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

Para inicializar a variável ArrayList da string de subtipo, você pode usar o modo de mapeamento JSON Dot Notation ou XPath. A captura de tela a seguir mostra como preencher uma variável ArrayList chamada CustomRoutes usando a notação JSON Dot. Verifique se você está apontando para um elemento em um objeto de matriz, como mostrado na captura de tela abaixo. Estamos preenchendo a ArrayList CustomRoutes com os nomes do objeto de matriz managerchain.
A ArrayList CustomRoutes é então usada para preencher as Rotas no componente AssignTask
![customroute](assets/arraylist.jpg)
Depois que a variável CustomRoutes ArrayList é inicializada com os valores dos dados enviados, as Rotas do componente AssignTask são preenchidas usando a variável CustomRoutes. A captura de tela abaixo mostra os roteiros personalizados em uma AssignTask
![asingtask](assets/customactions.jpg)

Para testar esse fluxo de trabalho em seu sistema, siga as etapas a seguir

* Baixe e salve o arquivo ArrayListVariable.zip no sistema de arquivos
* [Importar o arquivo zip](assets/arraylistvariable.zip) uso do gerenciador de pacotes AEM
* [Abra o formulário TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Insira algumas despesas e os nomes dos 2 gerentes
* Clique no botão enviar
* [Abra sua caixa de entrada](http://localhost:4502/aem/inbox)
* Você deve ver uma nova tarefa chamada &quot;Atribuir ao administrador de despesas&quot;
* Abrir o formulário associado à tarefa
* Você deve ver duas rotas personalizadas com os nomes do gerente
  [Explore o ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Esse fluxo de trabalho usa a variável ArrayList, a variável do tipo JSON e o editor de regras no componente Or-Split
