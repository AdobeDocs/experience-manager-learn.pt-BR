---
title: Variáveis no fluxo de trabalho AEM[Parte4]
seo-title: Variáveis no fluxo de trabalho AEM[Parte4]
description: Uso de variáveis do tipo xml,json,arraylist,documento no fluxo de trabalho aem
seo-description: Uso de variáveis do tipo xml,json,arraylist,documento no fluxo de trabalho aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Variável ArrayList no fluxo de trabalho AEM

Variáveis do tipo ArrayList foram introduzidas no AEM Forms 6.5. Um caso de uso comum para usar a variável ArrayList é definir rotas personalizadas a serem usadas em AssignTask.

Para usar a variável ArrayList em um fluxo de trabalho AEM, é necessário criar um formulário adaptável que gera elementos repetitivos nos dados enviados. Uma prática comum é definir um schema que contenha um elemento de matriz. Para a finalidade deste artigo, criei um schema JSON simples contendo elementos de matriz. O caso de uso é de um funcionário que preenche um relatório de despesas. No relatório de despesas, capturamos o nome do gerente do emissor e o nome do gerente do gerente. Os nomes do gerente são armazenados em um storage chamado management chain. A captura de tela abaixo mostra o formulário de relatório de despesas e os dados do envio para a Adaptive Forms.

![precário](assets/expensereport.jpg)

A seguir estão os dados do envio do formulário adaptável. O formulário adaptativo era baseado no schema JSON, os dados vinculados ao schema eram armazenados sob o elemento de dados do elemento afBoundData. A cadeia de gerenciamento é uma matriz e precisamos preencher a ArrayList com o elemento name do objeto dentro da matriz da cadeia de gerenciamento.

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

Para inicializar a variável ArrayList da string de subtipo, é possível usar o modo de mapeamento JSON Dot Notation ou XPath. A seguinte captura de tela mostra o preenchimento de uma variável ArrayList chamada CustomRoutes usando a Notação de ponto JSON. Verifique se você está apontando para um elemento em um objeto de matriz, como mostrado na captura de tela abaixo. Estamos preenchendo a CustomRoutes ArrayList com os nomes do objeto de matriz managementchain.
A CustomRoutes ArrayList é então usada para preencher as Rotas no componente AssignTask
![customroute](assets/arraylist.jpg)
Quando a variável CustomRoutes ArrayList for inicializada com os valores dos dados enviados, as Rotas do componente AssignTask serão então preenchidas usando a variável CustomRoutes. A captura de tela abaixo mostra as rotas personalizadas em uma AssignTask
![asingtask](assets/customactions.jpg)

Para testar esse fluxo de trabalho no sistema, siga as etapas a seguir

* Baixe e salve o arquivo ArrayListVariable.zip em seu sistema de arquivos
* [Importe o arquivo zip ](assets/arraylistvariable.zip) usando o Gerenciador de pacotes AEM
* [Abrir o formulário TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Insira algumas despesas e os 2 nomes dos gerentes
* Pressione o botão Enviar
* [Abrir sua caixa de entrada](http://localhost:4502/aem/inbox)
* Você deve ver uma nova tarefa intitulada &quot;Atribuir ao administrador de despesas&quot;
* Abra o formulário associado à tarefa
* Você deve ver duas rotas personalizadas com os nomes do gerente
   [Explore o ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Esse fluxo de trabalho usa a variável ArrayList, a variável do tipo JSON, o editor de regras no componente Or-Split
