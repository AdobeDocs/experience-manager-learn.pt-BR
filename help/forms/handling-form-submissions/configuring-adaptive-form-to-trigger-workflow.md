---
title: Configuração do formulário adaptável para acionar o fluxo de trabalho do AEM
description: Configure as opções de carga útil ao acionar o fluxo de trabalho do AEM no envio do formulário
sub-product: formulários
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 5%

---


# Configuração do formulário adaptável para acionar o fluxo de trabalho do AEM

## Pré-requisitos

O formulário de amostra usado nesse fluxo de trabalho é baseado em um modelo de formulário adaptável personalizado que precisa ser importado para o servidor AEM. O formulário de amostra fornecido precisa ser importado após a importação do template.

### Obter os modelos de formulário adaptáveis

* Baixe [Modelo de formulário adaptável](assets/af-form-template.zip)
* [Importar o modelo usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Fazer upload e instalar o modelo de formulário adaptável

### Obter o formulário adaptável de amostra

* Baixar [Formulário adaptável](assets/peak-application-form.zip)
* Navegue até [Formulário e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar -> Upload de arquivo
* O formulário adaptável de amostra será colocado em uma pasta chamada [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

O vídeo a seguir explica como configurar um formulário adaptável para acionar um fluxo de trabalho do AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

O vídeo a seguir mostra a carga do workflow e outros detalhes no repositório crx

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


