---
title: Configuração do formulário adaptável para acionar a visão geral do fluxo de trabalho do AEM
description: Configurar opções de carga ao acionar o fluxo de trabalho do AEM no envio do formulário
feature: Workflow
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5407
thumbnail: 40258.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 9f1dbd02-774a-4b84-90fa-02d4e468cbac
last-substantial-update: 2020-07-07T00:00:00Z
duration: 573
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 1%

---

# Configuração do formulário adaptável para acionar o fluxo de trabalho do AEM

## Pré-requisitos

O formulário de amostra usado neste fluxo de trabalho é baseado em um modelo de formulário adaptável personalizado que precisa ser importado para o servidor do AEM. O formulário de amostra fornecido precisa ser importado após a importação do template.

### Obter os modelos de formulário adaptável

* Baixar [Modelo de formulário adaptável](assets/af-form-template.zip)
* [Importar o modelo usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Fazer upload e instalar o modelo de formulário adaptável

### Obter o exemplo de formulário adaptável

* Baixar [Formulário adaptável](assets/peak-application-form.zip)
* Navegar até [Formulário e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar -> Upload de arquivo
* O exemplo de formulário adaptável é colocado em uma pasta chamada [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

O vídeo a seguir explica como configurar um Formulário adaptável para acionar um fluxo de trabalho do AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258?quality=12&learn=on)

O vídeo a seguir mostra a carga do fluxo de trabalho e outros detalhes no repositório crx

>[!VIDEO](https://video.tv.adobe.com/v/40259?quality=12&learn=on)
