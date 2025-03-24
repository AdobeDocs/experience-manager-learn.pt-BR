---
title: Enviar formulário headless para um serviço de envio personalizado
description: Personalizar sua resposta com base nos dados enviados
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 1%

---

# Personalizar resposta com base em dados enviados

Depois que o formulário for enviado, é importante fornecer feedback ao usuário sobre o resultado do envio. A resposta de envio pode incluir uma ID de transação ou simplesmente uma resposta personalizada. Para atender a esse caso de uso, um serviço de envio personalizado é escrito no AEM Forms e o formulário headless é enviado a esse serviço de envio personalizado.

## Pré-requisitos

Para implementar essa funcionalidade com êxito, é recomendável estar familiarizado com o seguinte

* Experiência com o Git
* Experiência com o AEM Cloud Manager
* Maven (este artigo foi testado com a versão 3.8.6)
* Instância de autor pronta para o AEM Forms Cloud local
* Acesso ao ambiente do AEM Forms as Cloud Service
* IntelliJ ou qualquer outro IDE


## Próximas etapas

[Escrever o serviço de envio personalizado](./custom-submit-service.md)
