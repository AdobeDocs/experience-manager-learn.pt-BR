---
title: Uso do AEM Forms com Adobe Sign
description: A Adobe Sign e a AEM Forms permitem automatizar transações complexas e incluir assinaturas eletrônicas legais como parte de uma experiência digital contínua.
feature: Adaptive Forms,Adobe Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Uso do AEM Forms com Adobe Sign

O Adobe Sign habilita fluxos de trabalho de assinatura eletrônica para formulários adaptáveis. As assinaturas eletrônicas melhoram os fluxos de trabalho para processar documentos para áreas legais, de vendas, de folha de pagamento, de gerenciamento de recursos humanos e muitas outras.
A integração entre o AEM Forms e o Adobe Sign permitirá que você faça o seguinte

* Use o Adaptive Forms para capturar dados e apresentar o Documento de Registro (DoR) gerado automaticamente para assinaturas
* Crie um Adaptive Forms com base no modelo PDF. Mesclar os dados com o modelo pdf e apresentar o mesmo para assinaturas
* Enviar documentos para assinatura usando o componente de fluxo de trabalho Assinar documento

## Pré-requisitos

Você precisa do seguinte para integrar o Adobe Sign com o AEM Forms:

* Um servidor AEM Forms habilitado para SSL
* Uma conta ativa de desenvolvedor do Adobe Sign.
* Um aplicativo de API do Adobe Sign
* Credenciais (ID do cliente e Segredo do cliente) do aplicativo da API do Adobe Sign.
