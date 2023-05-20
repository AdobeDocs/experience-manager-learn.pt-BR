---
title: Captura de mensagens de erro no serviço de modelo de dados de formulário como etapa no fluxo de trabalho
description: A partir do AEM Forms 6.5.1, agora temos a capacidade de capturar mensagens de erro geradas ao usar o serviço de modelo de dados de formulário de invocação como uma etapa no fluxo de trabalho de AEM. Fluxo de trabalho.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Captura de mensagens de erro na etapa do serviço Invoke Form Data Model

A partir do AEM Forms 6.5.1, agora temos a opção de capturar mensagens de erro e especificar opções de validação. A etapa Invocar serviço de modelo de dados de formulário foi aprimorada para fornecer os seguintes recursos.

* Fornecer uma opção para validação em três níveis (&quot;DESATIVADO&quot;, &quot;BÁSICO&quot; e &quot;COMPLETO&quot;) para lidar com exceções encontradas ao chamar o serviço de modelo de dados de formulário. As 3 opções indicam sucessivamente uma versão mais rigorosa da verificação dos requisitos específicos do banco de dados.
   ![níveis de validação](assets/validation-level.PNG)

* Fornecer uma caixa de seleção para personalizar a execução do workflow. Portanto, o usuário agora tem a flexibilidade de seguir em frente com a Execução do fluxo de trabalho, mesmo que a etapa Chamar modelo de dados de formulário acione Exceções.

* Armazenando informações importantes de Ocorrência de erros devido a exceções de validação. Três seletores de variável do tipo Autocomplete foram incorporados para selecionar variáveis relevantes para armazenar ErrorCode(String), ErrorMessage(String) e ErrorDetails(JSON). No entanto, ErrorDetails seria definido como nulo caso a exceção não seja DermisValidationException.
   ![captura de mensagens de erro](assets/fdm-error-details.PNG)

Com essas alterações, a etapa Chamar serviço do modelo de dados de formulário garante que os valores de entrada sigam as restrições de dados fornecidas no arquivo swagger. Por exemplo, a seguinte mensagem de erro é emitida quando os valores accountId e balance não são compatíveis com as restrições de dados especificadas no arquivo swagger.

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
