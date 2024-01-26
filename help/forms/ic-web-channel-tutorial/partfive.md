---
title: Criação de fragmentos de documento para manter o nome e o endereço do destinatário
description: Esta é a parte 5 de um tutorial de várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, criaremos o fragmento do documento para manter o nome e o endereço do recipient.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 229
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# Criação de fragmentos de documento para manter o nome e o endereço do destinatário {#creating-document-fragments-to-hold-the-recipient-name-and-address}

Nesta parte, criaremos o fragmento do documento para manter o nome e o endereço do recipient.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Os fragmentos de documento contêm o conteúdo de texto de documentos de comunicação interativa. Esse conteúdo de texto pode ser um texto estático ou inserido dos valores subjacentes dos elementos do modelo de dados. Por exemplo Dear {name}, onde Dear é texto estático e {name} é o nome do elemento de dados do formulário. No tempo de execução, isso resolverá para Dear Gloria Rios ou Dear John Jacobs dependendo do valor do elemento name.

O editor de rich text é intuitivo o suficiente para um usuário empresarial criar texto e inserir elementos de dados de formulário. O editor de fragmento de documento tem a capacidade de formatar texto, especificar tipos e estilos de fonte, inserir caracteres especiais e criar hiperlinks.

O editor de fragmento de documento também tem a capacidade de inserir condições em linha no texto, conforme demonstrado nesta [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Verifique se os elementos do Modelo de dados de formulário inseridos em fragmentos de um documento são descendentes do elemento raiz. Por exemplo, neste caso de uso, verifique se os elementos do objeto Usuário selecionados são filhos do objeto de saldos

## Próximas etapas

[Criar documento de comunicação interativa](./partsix.md)