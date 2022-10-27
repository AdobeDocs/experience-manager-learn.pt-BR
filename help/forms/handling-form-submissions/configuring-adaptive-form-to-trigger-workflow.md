---
title: Configuração do formulário adaptável para acionar AEM visão geral do fluxo de trabalho
description: Configure as opções de carga útil ao acionar AEM fluxo de trabalho no envio do formulário
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---

# Configuração do formulário adaptável para acionar AEM fluxo de trabalho

## Pré-requisitos

O formulário de amostra usado nesse fluxo de trabalho é baseado em um modelo de formulário adaptável personalizado que precisa ser importado para o servidor de AEM. O formulário de amostra fornecido precisa ser importado após a importação do template.

### Obter os modelos de formulário adaptáveis

* Baixar [Modelo de formulário adaptável](assets/af-form-template.zip)
* [Importar o modelo usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Fazer upload e instalar o modelo de formulário adaptável

### Obter o formulário adaptável de amostra

* Baixar [Formulário adaptável](assets/peak-application-form.zip)
* Navegue até [Formulário E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar -> Upload de arquivo
* O formulário adaptável de amostra é colocado em uma pasta chamada [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

O vídeo a seguir explica como configurar um formulário adaptável para acionar um fluxo de trabalho de AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

O vídeo a seguir mostra a carga do workflow e outros detalhes no repositório crx

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)
