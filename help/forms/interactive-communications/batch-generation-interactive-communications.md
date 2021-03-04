---
title: Usando a API em lote para gerar documentos de comunicação interativa
description: Amostra de ativos para gerar documentos do canal de impressão usando API em lote
feature: comunicação interativa
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 2%

---


# API em lote

Você pode usar a API em lote para produzir várias comunicações interativas de um modelo. O modelo é uma comunicação interativa sem dados. A API em lote combina dados com um modelo para produzir uma comunicação interativa. A API é útil na produção em massa de comunicações interativas. Por exemplo, contas telefônicas, demonstrativos de cartão de crédito para vários clientes.

[Saiba mais sobre a API de geração de lote](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Este artigo fornece ativos de amostra para gerar documentos de Comunicações interativas usando a API em lote.

## Geração de lote usando a pasta assistida

* Importe o [Modelo de comunicação interativa](assets/Beneficiaries-confirmation.zip) para o servidor do AEM Forms.
* Importe o [configuração de pasta observada](assets/batch-generation-api.zip). Isso criará uma pasta chamada `batchAPI` na unidade C.

**Se você estiver executando o AEM Forms em sistemas operacionais não-Windows, siga as 3 etapas mencionadas abaixo:**

1. [Abrir pasta assistida](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selecione BatchAPIWatchedFolder e clique em Editar.
3. Altere o Caminho para corresponder ao seu sistema operacional.

![path](assets/watched-folder-batch-api-basic.PNG)

* Baixe e extraia o conteúdo de [zip file](assets/jsonfile.zip). O arquivo zip contém a pasta chamada `jsonfile` que contém o arquivo `beneficiaries.json`. Esse arquivo tem os dados para gerar 3 documentos.

* Solte a pasta `jsonfile` na pasta de entrada da pasta assistida.
* Depois que a pasta for selecionada para processamento, verifique a pasta de resultados da pasta assistida. Você deve ver 3 arquivos PDF gerados

## Geração de lote usando solicitações REST

Você pode chamar a [API em lote](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) por meio de solicitações REST. Você pode expor endpoints REST para outros aplicativos para chamar a API para gerar documentos.
A amostra de ativos fornecida expõe o terminal REST para gerar documentos de Comunicação Interativa. O servlet aceita os seguintes parâmetros:

* fileName - Localização do arquivo de dados no sistema de arquivos.
* templatePath - Caminho do modelo IC
* saveLocation - Local para salvar os documentos gerados no sistema de arquivos
* channelType - Print,Web ou ambos
* recordId - caminho JSON para elemento para definir o nome de uma comunicação interativa

A captura de tela a seguir mostra os parâmetros e seus valores
![solicitação de amostra](assets/generate-ic-batch-servlet.PNG)

## Implante ativos de amostra no seu servidor

* Importe [ICTemplate](assets/ICTemplate.zip) usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [Manipulador de Envio Personalizado](assets/BatchAPICustomSubmit.zip) usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe [Formulário adaptativo](assets/BatchGenerationAPIAF.zip) usando o [Forms e a interface de documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implante e inicie [Pacote OSGI Personalizado](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) usando [Console Web Felix](http://localhost:4502/system/console/bundles)
* [Acionar geração de lote enviando o formulário](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
