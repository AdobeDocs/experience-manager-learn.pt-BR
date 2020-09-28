---
title: Criação de fragmentos de Documento para manter o nome e o endereço do recipient
seo-title: Criação de fragmentos de Documento para manter o nome e o endereço do recipient
description: 'Esta é a parte 5 de um tutorial de várias etapas para criar seu primeiro documento de comunicação interativo. Nesta parte, criaremos um fragmento de documento para manter o nome e o endereço do recipient. '
seo-description: 'Esta é a parte 5 de um tutorial de várias etapas para criar seu primeiro documento de comunicação interativo. Nesta parte, criaremos um fragmento de documento para manter o nome e o endereço do recipient. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---


# Criação de fragmentos de Documento para manter o nome e o endereço do recipient {#creating-document-fragments-to-hold-the-recipient-name-and-address}

Nesta parte, criaremos um fragmento de documento para manter o nome e o endereço do recipient.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Os fragmentos de documento contêm o conteúdo de texto dos documentos de comunicação interativos. Esse conteúdo de texto pode ser texto estático ou inserido a partir dos valores subjacentes dos elementos do modelo de dados. Por exemplo, Prezado {name}, onde Preto é texto estático e {name} é o nome do elemento de dados do formulário. No tempo de execução, isso resolverá para a Prezada Gloria Rios ou para o Prezado John Jacobs dependendo do valor do elemento name.

O editor de Rich Text é intuitivo o suficiente para que um usuário corporativo crie textos e insira elementos de dados do formulário. O editor de fragmentos de documento tem a capacidade de formatar texto, especificar tipos de fonte e estilos, inserir caracteres especiais e criar hiperlinks.

O editor de fragmentos de documento também tem a capacidade de inserir condições em linha no seu texto, conforme demonstrado neste [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Certifique-se de que os elementos do Modelo de dados de formulário inseridos em um fragmento de documento sejam descendentes do elemento raiz. Por exemplo, nesse caso de uso, verifique se os elementos do objeto Usuário que você selecionar são filhos do objeto de saldos

