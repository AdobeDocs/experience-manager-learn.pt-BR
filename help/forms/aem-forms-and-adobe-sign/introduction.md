---
title: Uso do AEM Forms com Adobe Sign
description: A Adobe Sign e a AEM Forms permitem automatizar transações complexas e incluir assinaturas eletrônicas legais como parte de uma experiência digital contínua.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Uso do AEM Forms com Adobe Sign

A Adobe Sign habilita workflows de assinatura eletrônica para formulários adaptáveis. As assinaturas eletrônicas melhoram os workflows para processar documentos para áreas legais, de vendas, de folha de pagamento, de gerenciamento de recursos humanos e muitas outras áreas.
A integração entre a AEM Forms e a Adobe Sign permitirá que você faça o seguinte:

* Use a Forms adaptativa para capturar dados e apresentar Documento de Record (DoR) gerado automaticamente para assinaturas
* Crie um Forms adaptável com base no seu modelo de PDF. Mesclar os dados com o modelo pdf e apresentar o mesmo para assinaturas
* Enviar documentos para assinatura usando o componente de fluxo de trabalho Assinar Documento

## Pré-requisitos

É necessário o seguinte para integrar o Adobe Sign à AEM Forms:

* Um servidor AEM Forms habilitado para SSL
* Uma conta de desenvolvedor Adobe Sign ativa.
* Um aplicativo de API da Adobe Sign
* Credenciais (ID do cliente e segredo do cliente) do aplicativo da API Adobe Sign.

