---
title: Reagir aplicativo com AEM Forms e Acrobat Sign
description: A Acrobat Sign e a AEM Forms permitem automatizar transações complexas e incluir assinaturas eletrônicas legais como parte de uma experiência digital contínua.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# AEM Forms com Acrobat Sign Web Form


Este tutorial aborda o caso de uso da geração de um documento de comunicação interativo com os dados enviados do [Reagir](https://react.dev/) aplicativo e apresentação do documento gerado para assinatura usando o formulário web Acrobat Sign.

Veja a seguir o fluxo do caso de uso

* O usuário preenche um formulário no aplicativo React.
* Os dados do formulário são enviados para um endpoint da AEM Forms para gerar um documento de comunicações interativas.
* Crie o url do widget do Acrobat Sign usando o documento gerado.
* Apresente o url do widget no aplicativo que faz a chamada para que o usuário assine o documento.

## Pré-requisitos

Você precisará do seguinte para que o caso de uso funcione:

* Um servidor AEM com o pacote de complementos do Forms
* Um [chave de integração para um aplicativo Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)
