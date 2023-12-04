---
title: Criar um componente para listar os dados de formulário
description: Tutorial para criar o componente de resumo para revisar os dados do formulário antes do envio.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 49
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# Criar componente para resumir os dados de formulário

Um componente simples foi criado para listar os dados de formulário para revisão. A variável [função de visita da API do guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) foi usado para iterar pelos campos de formulário. O código na biblioteca do cliente associado a este componente obtém os componentes painel/tabela no formulário. Dos elementos secundários deste painel/tabela, os componentes dos campos de formulário, título, valor e expressão SOM são extraídos usando os métodos da API GuidBridge. Uma tabela de HTML simples é então construída com o título, valor e expressão SOM para o usuário final revisar/editar os dados do formulário antes de enviar o formulário.

Por exemplo, a captura de tela abaixo mostra a tabela criada para listar os campos e seus valores do **SeusDetalhes**. O último TD no TR é usado para editar o valor do campo usando a expressão SOM dos campos.

![visit-func](assets/visit-function.png)

## Próximas etapas

[Testar a solução no sistema local](./deploy-on-your-system.md)
