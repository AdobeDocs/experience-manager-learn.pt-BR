---
title: Capturando mensagens de erro no serviço do Modelo de dados de formulário como etapa no fluxo de trabalho
description: A partir do AEM Forms 6.5.1, agora temos a capacidade de capturar mensagens de erro geradas ao usar invocar o serviço de modelo de dados de formulário como uma etapa AEM fluxo de trabalho. Fluxo de trabalho.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Capturando mensagens de erro na etapa Chamar serviço do modelo de dados de formulário

A partir do AEM Forms 6.5.1, agora temos a opção de capturar mensagens de erro e especificar opções de validação. A etapa Chamar serviço do Modelo de dados de formulário foi aprimorada para fornecer os seguintes recursos.

* Fornecendo uma opção para validação de 3 camadas (&quot;OFF&quot;, &quot;BASIC&quot; e &quot;FULL&quot;) para lidar com Exceções encontradas ao invocar o Serviço de Modelo de Dados de Formulário. As 3 opções indicam sucessivamente uma versão mais estrita de verificação dos requisitos específicos do banco de dados.
   ![níveis de validação](assets/validation-level.PNG)

* Fornecer uma caixa de seleção para personalizar a execução do fluxo de trabalho. Portanto, o usuário agora tem a flexibilidade de prosseguir com a Execução do fluxo de trabalho, mesmo se a etapa Invocar modelo de dados de formulário acionar Exceções.

* Armazenando informações importantes sobre o erro que ocorre devido a exceções de validação. Três seletores de variável do tipo Autocomplete foram incorporados para selecionar variáveis relevantes para armazenar ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). No entanto, ErrorDetails seria definido como nulo, pois a exceção não é um DermisValidationException.
   ![como capturar mensagens de erro](assets/fdm-error-details.PNG)

Com essas alterações, a etapa Chamar serviço do modelo de dados de formulário garante que os valores de entrada sigam as restrições de dados fornecidas no arquivo do gerenciador. Por exemplo, a seguinte mensagem de erro é emitida quando os valores accountId e balance não estão em conformidade com as restrições de dados especificadas no arquivo swagger.

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
