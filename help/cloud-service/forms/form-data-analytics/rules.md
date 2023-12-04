---
title: Relatório sobre campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS com o Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 62
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Definir a regra

Na propriedade Tags, criamos dois novos [regras](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Erro de validação de campo e FormSubmit**).

![formulário adaptável](assets/rules.png)


## Erro de validação de campo

A variável **Erro de validação de campo** a regra é acionada sempre que há erro de validação no campo de formulário adaptável. Por exemplo, em nosso formulário, se o número de telefone ou o email não estiver no formato esperado, uma mensagem de erro de validação será exibida.

A regra Erro de validação de campo é configurada definindo o evento como _**Adobe Experience Manager Forms-Error**_ como mostrado na captura de tela



![requerente-Estado-residência](assets/field_validation_error_rule.png)

O Adobe Analytics - Definir variáveis é configurado da seguinte maneira

![definir ação](assets/field_validation_action_rule.png)

## Regra de envio de formulário

A regra de Envio de formulário é acionada toda vez que um Formulário adaptável é enviado com êxito.

A regra de envio de formulário é configurada usando o _**Adobe Experience Manager Forms - Enviar**_ evento

![form-submit-rule](assets/form-submit-rule.png)

Na regra Enviar formulário, o valor do elemento de dados _**CandidatosEstadoDeResidência**_ é mapeado para prop5 e o valor do elemento de dados FormTitle é mapeado para prop8.

As variáveis Adobe Analytics - Set são configuradas da seguinte maneira.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Quando estiver pronto para testar o código de tags,[publicar as alterações feitas nas tags](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) usando o fluxo de publicação.

## Próximas etapas

[Testar a solução](./test.md)