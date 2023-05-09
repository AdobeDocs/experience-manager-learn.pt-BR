---
title: Criar um componente para listar os dados do formulário
description: Tutorial para criar o componente de resumo para revisar os dados do formulário antes de enviar.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Criar componente para resumir os dados do formulário

Um componente simples foi criado para listar os dados do formulário para revisão. O [função de visita da API do guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) foi usada para iterar pelos campos do formulário. O código na biblioteca de clientes associado a esse componente obtém os componentes de painel/tabela no formulário. A partir dos elementos filho desse painel/componentes da tabela, o título dos campos do formulário, o valor e a expressão SOM são extraídos usando os métodos da API GuidBridge. Uma tabela de HTML simples é então construída com o título, o valor e a expressão SOM para que o usuário final revise/edite os dados do formulário antes de enviar o formulário.

Por exemplo, a captura de tela abaixo mostra a tabela criada para listar os campos e seus valores da variável **Seus detalhes**. O último TD no TR é usado para editar o valor do campo usando os campos da expressão SOM.

![visit-func](assets/visit-function.png)

## Próximas etapas

[Teste a solução em seu sistema local](./deploy-on-your-system.md)
