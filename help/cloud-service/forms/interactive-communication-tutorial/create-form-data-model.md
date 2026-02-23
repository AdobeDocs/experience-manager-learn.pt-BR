---
title: Criar modelo de dados de formulário para o documento IC
description: Saiba como criar um modelo de dados de formulário no AEM Forms para recuperar dinamicamente dados para o documento de comunicação interativa.
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# Criar modelo de dados de formulário para o documento IC

Crie um Modelo de dados do Forms para integrar fontes de dados externas com a Comunicação interativa no Adobe AEM. Esse processo envolve configurar um serviço RESTful, fazer upload de um arquivo Swagger e configurar pontos de acesso de serviço para recuperar e vincular dados dinamicamente. Saiba como se conectar com segurança a serviços externos e testar o modelo para garantir uma recuperação de dados bem-sucedida.

Um servidor de API simulado foi implementado que simula o serviço Pedidos para fins de desenvolvimento e teste. Ele expõe um endpoint para buscar pedidos de um determinado usuário (por exemplo, por ID de usuário), retornando dados de pedido predefinidos ou gerados dinamicamente no mesmo schema que a API de produção.

O arquivo swagger usado na criação do modelo de dados de formulário pode ser [baixado daqui](assets/UsersAndOrders.json)

>[!VIDEO](https://video.tv.adobe.com/v/3480005/?learn=on&enablevpops)

## Próximas etapas

[Criar modelo](./create-template.md)
