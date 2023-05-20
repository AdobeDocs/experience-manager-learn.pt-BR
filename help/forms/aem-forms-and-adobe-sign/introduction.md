---
title: Utilização do AEM Forms com o Acrobat Sign
description: O Acrobat Sign e o AEM Forms permitem automatizar transações complexas e incluir assinaturas eletrônicas legais como parte de uma experiência digital contínua.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Utilização do AEM Forms com o Acrobat Sign

O Acrobat Sign habilita fluxos de trabalho de assinatura eletrônica para formulários adaptáveis. As assinaturas eletrônicas melhoram os fluxos de trabalho para processar documentos para áreas jurídicas, de vendas, de folha de pagamento, de gerenciamento de recursos humanos e muito mais.
A integração entre o AEM Forms e o Acrobat Sign permitirá fazer o seguinte

* Usar o Adaptive Forms para capturar dados e apresentar o Documento de registro (DoR) gerado automaticamente para assinaturas
* Crie um Forms adaptável com base no seu modelo de PDF. Mescle os dados com o modelo pdf e apresente o mesmo para assinaturas
* Enviar documentos para assinatura usando o componente de fluxo de trabalho Assinar documento

## Pré-requisitos

Você precisa do seguinte para integrar o Acrobat Sign ao AEM Forms:

* Um servidor AEM Forms habilitado para SSL
* Uma conta de desenvolvedor ativa do Acrobat Sign.
* Um aplicativo de API do Acrobat Sign
* Credenciais (ID do cliente e Segredo do cliente) do aplicativo da API do Acrobat Sign.
