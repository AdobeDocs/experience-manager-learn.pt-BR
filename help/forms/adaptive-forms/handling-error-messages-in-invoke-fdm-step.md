---
title: Capturando mensagens de erro no serviço do Modelo de dados de formulário como etapa no fluxo de trabalho
seo-title: Capturando mensagens de erro no serviço do Modelo de dados de formulário como etapa no fluxo de trabalho
description: A partir do AEM Forms 6.5.1, agora temos a capacidade de capturar mensagens de erro geradas ao usar invocar o Serviço de modelo de dados de formulário como uma etapa no fluxo de trabalho do AEM. Fluxo de trabalho.
seo-description: A partir do AEM Forms 6.5.1, agora temos a capacidade de capturar mensagens de erro geradas ao usar invocar o Serviço de modelo de dados de formulário como uma etapa no fluxo de trabalho do AEM. Fluxo de trabalho.
feature: Fluxo de trabalho
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 1%

---


# Capturando mensagens de erro na etapa Chamar serviço do modelo de dados de formulário

A partir do AEM Forms 6.5.1, agora temos a opção de capturar mensagens de erro e especificar opções de validação. A etapa Chamar serviço do Modelo de dados de formulário foi aprimorada para fornecer os seguintes recursos.

* Fornecendo uma opção para validação de 3 camadas (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) para lidar com Exceções encontradas ao invocar o Serviço de Modelo de Dados de Formulário. As 3 opções indicam sucessivamente uma versão mais estrita de verificação dos requisitos específicos do banco de dados.
   ![níveis de validação](assets/validation-level.PNG)

* Fornecer uma caixa de seleção para personalizar a execução do fluxo de trabalho. Portanto, o usuário agora terá a flexibilidade de prosseguir com a Execução do fluxo de trabalho, mesmo se a etapa Invocar modelo de dados de formulário acionar Exceções.

* Armazenando informações importantes sobre o erro que ocorre devido a exceções de validação. Três seletores de variável do tipo Autocomplete foram incorporados para selecionar variáveis relevantes para armazenar ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). No entanto, ErrorDetails seria definido como nulo, pois a exceção não é um DermisValidationException.
   ![como capturar mensagens de erro](assets/fdm-error-details.PNG)

Com essas alterações, a etapa Chamar serviço do modelo de dados de formulário garantirá que os valores de entrada sigam as restrições de dados fornecidas no arquivo do comutador. Por exemplo, a seguinte mensagem de erro será emitida quando os valores accountId e balance não estiverem em conformidade com as restrições de dados especificadas no arquivo swagger.

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


