---
title: Criação de Fragmentos de Documento para manter o nome e o endereço do recipient
seo-title: Criação de Fragmentos de Documento para manter o nome e o endereço do recipient
description: 'Esta é a parte 5 de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, criaremos fragmento de documento para manter o nome e o endereço do recipient. '
seo-description: 'Esta é a parte 5 de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, criaremos fragmento de documento para manter o nome e o endereço do recipient. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Comunicação interativa
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---


# Criando Fragmentos de Documento para manter o nome e o endereço do recipient {#creating-document-fragments-to-hold-the-recipient-name-and-address}

Nesta parte, criaremos fragmento de documento para manter o nome e o endereço do recipient.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Fragmentos de documento contêm o conteúdo de texto de documentos de comunicação interativos. Esse conteúdo de texto pode ser texto estático ou inserido a partir dos valores dos elementos subjacentes do modelo de dados. Por exemplo, Prezado {name}, onde Preto é texto estático e {name} é o nome do elemento de dados do formulário. No tempo de execução, isso resolverá para Querido Gloria Rios ou Caro John Jacobs, dependendo do valor do elemento name .

O editor de rich text é intuitivo o suficiente para um usuário empresarial criar texto e inserir elementos de dados do formulário. O editor de fragmentos de documento tem a capacidade de formatar o texto, especificar tipos e estilos de fonte, inserir caracteres especiais e criar hiperlinks.

O editor de fragmento de documento também tem a capacidade de inserir condições em linha no seu texto, conforme demonstrado neste [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Certifique-se de que os elementos do Modelo de dados de formulário inseridos em um fragmento de documento sejam descendentes do elemento raiz. Por exemplo, nesse caso de uso, verifique se os elementos do objeto Usuário selecionados são filhos do objeto de saldos

