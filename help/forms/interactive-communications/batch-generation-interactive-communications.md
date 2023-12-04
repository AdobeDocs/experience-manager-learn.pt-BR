---
title: Uso da API de lote para gerar documentos de comunicação interativa
description: Ativos de amostra para geração de documentos de canal de impressão usando a API em lote
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 109
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# API em lote

Você pode usar a API de lote para produzir várias comunicações interativas a partir de um modelo. O template é uma comunicação interativa sem dados. A API de lote combina dados com um modelo para produzir uma comunicação interativa. A API é útil na produção em massa de comunicações interativas. Por exemplo, contas de telefone, demonstrativos de cartão de crédito para vários clientes.

[Saiba mais sobre a API de geração em lote](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

Este artigo fornece ativos de amostra para gerar documentos de Comunicações interativas usando a API em lote.

## Geração de lote usando a pasta monitorada

* Importe o [Modelo de comunicação interativa](assets/Beneficiaries-confirmation.zip) no servidor do AEM Forms.
* Importe o [configuração da pasta monitorada](assets/batch-generation-api.zip). Isso criará uma pasta chamada `batchAPI` na unidade C.

**Se você estiver executando o AEM Forms em um sistema operacional que não seja Windows, siga as 3 etapas mencionadas abaixo:**

1. [Abrir pasta monitorada](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. Selecione BatchAPIWatchedFolder e clique em Editar.
3. Altere o Caminho para corresponder ao seu sistema operacional.

![caminho](assets/watched-folder-batch-api-basic.PNG)

* Baixe e extraia o conteúdo de [arquivo zip](assets/jsonfile.zip). O arquivo zip contém a pasta de nome `jsonfile` que contém `beneficiaries.json` arquivo. Esse arquivo tem os dados para gerar três documentos.

* Solte o `jsonfile` na pasta de entrada da sua pasta monitorada.
* Quando a pasta for selecionada para processamento, verifique a pasta de resultados da pasta monitorada. Você deve ver arquivos de 3 PDF gerados

## Geração de lote usando solicitações REST

Você pode chamar a variável [API em lote](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) por meio de solicitações REST. É possível expor endpoints REST para que outros aplicativos chamem a API para gerar documentos.
Os ativos de amostra fornecidos expõem o endpoint REST para gerar documentos de Comunicação interativa. O servlet aceita os seguintes parâmetros:

* fileName - Local do arquivo de dados no sistema de arquivos.
* templatePath - Caminho de modelo IC
* saveLocation - Local para salvar os documentos gerados no sistema de arquivos
* channelType - Impressão, Web ou ambos
* recordId - Caminho JSON para elemento, para definir o nome de uma comunicação interativa

A captura de tela a seguir mostra os parâmetros e seus valores
![solicitação de exemplo](assets/generate-ic-batch-servlet.PNG)

## Implantar ativos de amostra no servidor

* Importar [Modelo de TIC](assets/ICTemplate.zip) usar [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [Manipulador de envio personalizado](assets/BatchAPICustomSubmit.zip) usar [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar [Formulário adaptável](assets/BatchGenerationAPIAF.zip) usando o [Interface do Forms e do Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Implantar e iniciar [Pacote OSGi personalizado](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) usar [Felix web console](http://localhost:4502/system/console/bundles)
* [Acione a Geração de lote enviando o formulário](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
