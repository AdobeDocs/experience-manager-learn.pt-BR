---
title: Como capturar mensagens de erro no serviço de modelo de dados de formulário como etapa no fluxo de trabalho
seo-title: Como capturar mensagens de erro no serviço de modelo de dados de formulário como etapa no fluxo de trabalho
description: A partir do AEM Forms 6.5.1, agora temos a capacidade de capturar mensagens de erro geradas ao usar chamar o serviço de modelo de dados de formulário como uma etapa AEM fluxo de trabalho. Fluxo de trabalho.
seo-description: A partir do AEM Forms 6.5.1, agora temos a capacidade de capturar mensagens de erro geradas ao usar chamar o serviço de modelo de dados de formulário como uma etapa AEM fluxo de trabalho. Fluxo de trabalho.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# Captura de mensagens de erro na etapa Chamar serviço de modelo de dados de formulário

A partir do AEM Forms 6.5.1, agora temos a opção de capturar mensagens de erro e especificar opções de validação. A etapa Chamar serviço de modelo de dados de formulário foi aprimorada para fornecer os seguintes recursos.

* Fornecendo uma opção para validação de 3 camadas (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) para lidar com Exceções encontradas ao invocar o Serviço de Modelo de Dados de Formulário. As 3 opções denotam sucessivamente uma versão mais estrita da verificação de requisitos específicos do banco de dados.
   ![níveis de validação](assets/validation-level.PNG)

* Fornecer uma caixa de seleção para personalizar a execução do Fluxo de trabalho. Portanto, o usuário agora terá a flexibilidade de prosseguir com a Execução do fluxo de trabalho, mesmo se a etapa Invocar modelo de dados do formulário lançar Exceções.

* Armazenamento de informações importantes sobre erros que ocorrem devido a exceções de validação. Três seletores de variável de tipo de preenchimento automático foram incorporados para selecionar variáveis relevantes para armazenar ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). No entanto, ErrorDetails seria definido como nulo se a exceção não fosse um DermisValidationException.
   ![como capturar mensagens de erro](assets/fdm-error-details.PNG)

Com essas alterações, a etapa Chamar serviço de modelo de dados de formulário garantirá que os valores de entrada atendam às restrições de dados fornecidas no arquivo swagger. Por exemplo, a seguinte mensagem de erro será emitida quando a accountId e os valores de balanço não estiverem em conformidade com as restrições de dados especificadas no arquivo swagger.

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


