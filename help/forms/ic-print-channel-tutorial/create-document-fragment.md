---
title: Criação de fragmento de Documento
description: 'Esta é a parte 5 de um tutorial de várias etapas para criar seu primeiro documento de comunicação interativo. Nesta parte, criaremos um fragmento de documento para manter o nome e o endereço do recipient. '
seo-description: 'Esta é a parte 5 de um tutorial de várias etapas para criar seu primeiro documento de comunicação interativo. Nesta parte, criaremos um fragmento de documento para manter o nome e o endereço do recipient. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# Criação de fragmento de Documento

Nesta parte, criaremos um fragmento de documento para manter o nome e o endereço do recipient.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Os fragmentos de documento contêm o conteúdo de texto dos documentos de comunicação interativos. Esse conteúdo de texto pode ser texto estático ou inserido a partir dos valores subjacentes dos elementos do modelo de dados. Por exemplo **Prezado _{name}_**, em que Preto é texto estático e o nome é o nome do elemento do modelo de dados de formulário. No tempo de execução, isso será resolvido para **Prezada Gloria Rios**ou **Prezado John Jacobs**, dependendo do valor do elemento name.

O Editor de Rich Text é intuitivo o suficiente para que um usuário corporativo crie textos e insira elementos de dados do formulário. O editor de fragmentos de documento tem a capacidade de formatar texto, especificar tipos de fonte e estilos, inserir caracteres especiais e criar hiperlinks.

O editor de fragmentos de documento também tem a capacidade de inserir condições embutidas em seu texto, conforme demonstrado neste [vídeo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Certifique-se de que os elementos do Modelo de dados de formulário inseridos em um fragmento de documento sejam descendentes do elemento raiz. Por exemplo, nesse caso de uso, verifique se os elementos do objeto Usuário que você selecionar são filhos do objeto de saldos

