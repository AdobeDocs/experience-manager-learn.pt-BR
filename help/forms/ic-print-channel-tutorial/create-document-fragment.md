---
title: Criação de fragmento de documento
description: Esta é a parte 5 de um tutorial de várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, criaremos o fragmento do documento para manter o nome e o endereço do recipient.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 220
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# Criação de fragmento de documento

Nesta parte, criaremos o fragmento do documento para manter o nome e o endereço do recipient.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Os fragmentos de documento contêm o conteúdo de texto de documentos de comunicação interativa. Esse conteúdo de texto pode ser um texto estático ou inserido dos valores subjacentes dos elementos do modelo de dados. Por exemplo **Prezado(a) _{name}_**, em que Dear é texto estático e name é o nome do elemento do modelo de dados de formulário. No tempo de execução, isso resolverá para **Querida Gloria Rios**ou **Caro John Jacobs**dependendo do valor do elemento name.

O editor de rich text é intuitivo o suficiente para um usuário empresarial criar texto e inserir elementos de dados de formulário. O editor de fragmento de documento tem a capacidade de formatar texto, especificar tipos e estilos de fonte, inserir caracteres especiais e criar hiperlinks.

O editor de fragmento de documento também tem a capacidade de inserir condições em linha no texto, conforme demonstrado nesta [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Verifique se os elementos do Modelo de dados de formulário inseridos em fragmentos de um documento são descendentes do elemento raiz. Por exemplo, neste caso de uso, verifique se os elementos do objeto Usuário selecionados são filhos do objeto de saldos

## Próximas etapas

[Criar documento de canal de impressão](./create-print-channel-document.md)
