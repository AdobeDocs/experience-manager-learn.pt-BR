---
title: Aplicativo React com AEM Forms e Acrobat Sign
description: O Acrobat Sign e o AEM Forms permitem automatizar transações complexas e incluir assinaturas eletrônicas legais como parte de uma experiência digital contínua.
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# AEM Forms com formulário web do Acrobat Sign


Este tutorial o guiará pelo caso de uso de geração de um documento de comunicação interativa com os dados enviados pelo aplicativo [React](https://react.dev/) e de apresentação do documento gerado para assinatura usando o formulário web do Acrobat Sign.

Veja a seguir o fluxo do caso de uso

* O usuário preenche um formulário no aplicativo React.
* Os dados de formulário são enviados a um endpoint da AEM Forms para gerar um documento de comunicações interativo.
* Crie o URL do widget do Acrobat Sign usando o documento gerado.
* Apresentar o url do widget no aplicativo de chamada para que o usuário assine o documento.

## Pré-requisitos

Você precisará do seguinte para que o caso de uso funcione:

* Um servidor do AEM com pacote complementar do Forms
* Uma [chave de integração para um aplicativo do Acrobat Sign](https://helpx.adobe.com/br/sign/kb/how-to-create-an-integration-key.html)

## Próximas etapas

Escreva um [serviço OSGi personalizado para gerar o Documento de comunicação interativa](./create-ic-document.md) usando a API documentada
