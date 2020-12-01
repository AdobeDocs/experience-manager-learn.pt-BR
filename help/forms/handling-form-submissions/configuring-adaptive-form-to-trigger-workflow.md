---
title: Configuração do formulário adaptável para acionar AEM fluxo de trabalho
description: Configurar opções de carga ao acionar AEM fluxo de trabalho no envio do formulário
sub-product: formulários
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 5407
thumbnail: 40258.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 0%

---


# Configuração do formulário adaptável para acionar AEM fluxo de trabalho

## Pré-requisitos

O formulário de amostra usado neste fluxo de trabalho é baseado em um modelo de formulário adaptável personalizado que precisa ser importado para o servidor AEM. O formulário de amostra fornecido precisa ser importado após a importação do modelo.

### Obter os modelos de formulário adaptáveis

* Baixar [Modelo de Formulário Adaptável](assets/af-form-template.zip)
* [Importar o modelo usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Carregar e instalar o modelo de formulário adaptativo

### Obtenha o formulário adaptável de amostra

* Baixar [Formulário adaptável](assets/peak-application-form.zip)
* Navegue até [Formulário e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em Criar -> Upload de arquivo
* O formulário adaptável de amostra será colocado em uma pasta chamada [Application Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments/applicationforms)

O vídeo a seguir explica como configurar um formulário adaptável para acionar um fluxo de trabalho AEM
>[!VIDEO](https://video.tv.adobe.com/v/40258/?quality=9&learn=on)

O vídeo a seguir mostra a carga do fluxo de trabalho e outros detalhes no repositório do crx

>[!VIDEO](https://video.tv.adobe.com/v/40259/?quality=9&learn=on)


