---
title: Implementar a funcionalidade de salvar e retomar para um formulário adaptável
description: Saiba como armazenar e recuperar dados de formulário adaptáveis da conta de armazenamento do Azure.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# Introdução

Neste tutorial, implementaremos um caso de uso simples que permite que os preenchimentos de formulário salvem e retomem o processo de preenchimento de formulário. O fluxo do caso de uso é o seguinte

* O usuário começa a preencher um formulário adaptável.
* O usuário salva o formulário e deseja continuar o preenchimento do formulário em uma data posterior.
* O usuário recebe uma notificação por email com um link para o formulário salvo.

## Pré-requisitos

* Experiência com o AEM Forms CS, especialmente na criação de modelos de dados de formulário.
* Experiência na implantação de código usando o Cloud Manager.
* Acesso à instância pronta para nuvem do AEM Forms CS.

Para implementar o caso de uso acima no AEM Forms CS, você precisará do seguinte

* [Instância pronta para nuvem do AEM Forms CS](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Conta do portal do Azure](https://portal.azure.com/)
* [Conta SendGrid](https://sendgrid.com/)

### Próximas etapas

[Criar componente de página](./page-component.md)

