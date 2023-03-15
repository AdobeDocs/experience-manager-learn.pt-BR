---
title: Relatar campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS ao Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Definir a regra

Na propriedade Tags , criamos 2 novos [regras](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Erro de validação de campo e FormSubmit**).

![forma adaptável](assets/rules.png)


## Erro de validação do campo

O **Erro de validação do campo** a regra é acionada sempre que há um erro de validação no campo de formulário adaptável. Por exemplo, em nosso formulário, se o número de telefone ou o email não estiver no formato esperado, uma mensagem de erro de validação será exibida.

A regra de Erro de Validação de Campo é configurada definindo o evento como _**Adobe Experience Manager Forms-Error**_ como mostrado na captura de tela



![requerente-Estado-residência](assets/field_validation_error_rule.png)

Adobe Analytics - Definir variáveis é configurado da seguinte maneira

![definir ação](assets/field_validation_action_rule.png)

## Regra de envio de formulário

A regra de envio de formulário é acionada sempre que um formulário adaptável é enviado com êxito.

A regra de envio de formulário é configurada com o uso da variável _**Adobe Experience Manager Forms - Enviar**_ evento

![regra de envio de formulário](assets/form-submit-rule.png)

Na regra de envio de formulário, o valor do elemento de dados _**RequerentesEstadoDeResidência**_ é mapeado para prop5 e o valor do elemento de dados FormTitle é mapeado para prop8.

As variáveis Adobe Analytics - Definir são configuradas da seguinte maneira.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Quando estiver pronto para testar seu código de Tags,[publicar as alterações feitas nas tags](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) usando o fluxo de publicação.
