---
title: Aplicativo React com AEM Forms e Acrobat Sign
description: O Acrobat Sign e o AEM Forms permitem automatizar transações complexas e incluir assinaturas eletrônicas legais como parte de uma experiência digital contínua.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 2ff7be5b-884c-420d-9a06-f0e2a99d3ef3
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# AEM Forms com formulário web do Acrobat Sign


Este tutorial aborda o caso de uso da geração de um documento de comunicação interativa com os dados enviados do [React](https://react.dev/) e apresentar o documento gerado para assinatura usando o formulário web do Acrobat Sign.

Veja a seguir o fluxo do caso de uso

* O usuário preenche um formulário no aplicativo React.
* Os dados de formulário são enviados a um endpoint da AEM Forms para gerar um documento de comunicações interativo.
* Crie o URL do widget do Acrobat Sign usando o documento gerado.
* Apresentar o url do widget no aplicativo de chamada para que o usuário assine o documento.

## Pré-requisitos

Você precisará do seguinte para que o caso de uso funcione:

* Um servidor AEM com pacote complementar do Forms
* Um [chave de integração para um aplicativo do Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## Próximas etapas

Escrever um [serviço OSGi personalizado para gerar o Documento de comunicação interativa](./create-ic-document.md) uso da API documentada
