---
title: Relatório sobre campos de dados de formulário enviados usando o Adobe Analytics
description: Integrar o AEM Forms CS com o Adobe Analytics para criar relatórios sobre campos de dados de formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Definir a regra

Na propriedade Tags, criamos duas novas [regras](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Erro de validação de campo e FormSubmit**).

![formulário-adaptável](assets/rules.png)


## Erro de validação de campo

A regra **Erro de validação de campo** é acionada sempre que há erro de validação no campo de formulário adaptável. Por exemplo, em nosso formulário, se o número de telefone ou o email não estiver no formato esperado, uma mensagem de erro de validação será exibida.

A regra Erro de Validação de Campo é configurada definindo o evento como _&#x200B;**Adobe Experience Manager Forms-Error**&#x200B;_ conforme mostrado na captura de tela



![requerente-de-residência](assets/field_validation_error_rule.png)

O Adobe Analytics - Definir variáveis é configurado da seguinte maneira

![definir ação](assets/field_validation_action_rule.png)

## Regra de envio de formulário

A regra de Envio de formulário é acionada toda vez que um Formulário adaptável é enviado com êxito.

A regra de Envio de Formulário é configurada usando o evento _&#x200B;**Adobe Experience Manager Forms - Enviar**&#x200B;_

![regra de envio de formulário](assets/form-submit-rule.png)

Na regra de Envio de Formulário, o valor do elemento de dados _&#x200B;**ApplicantsStateOfResidence**&#x200B;_ é mapeado para prop5 e o valor do elemento de dados FormTitle é mapeado para prop8.

As variáveis Adobe Analytics - Set são configuradas da seguinte maneira.
![formulário-envio-regra-conjunto-variáveis](assets/form-submit-set-variable.png)

Quando estiver pronto para testar seu código de marcas,[publique as alterações feitas nas marcas](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) usando o fluxo de publicação.

## Próximas etapas

[Testar a solução](./test.md)