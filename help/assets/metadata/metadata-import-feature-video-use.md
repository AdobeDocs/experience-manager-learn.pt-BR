---
title: Usando importação e exportação de metadados no AEM Assets
description: Os recursos de importação e exportação de metadados do AEM Assets permitem que os autores de conteúdo movam facilmente os metadados de ativos para dentro e para fora do AEM e aproveitem o poder do Microsoft Excel para manipular metadados em escala, facilitando os metadados de atualização em massa dos ativos existentes no AEM.
topics: metadata
audience: all
doc-type: feature video
activity: use
kt: 647
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 2c0818d0223a3db55e6407068f4802b9e7f7dd83
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 3%

---


# Usando importação e exportação de metadados no AEM Assets{#using-metadata-import-and-export-in-aem-assets}

Os recursos de importação e exportação de metadados do AEM Assets permitem que os autores de conteúdo movam facilmente os metadados de ativos para dentro e para fora do AEM e aproveitem o poder do Microsoft Excel para manipular metadados em escala, facilitando os metadados de atualização em massa dos ativos existentes no AEM.

## Exportação de metadados {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Importação de metadados {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

Baixar [pasta de esportes WeRetail](assets/we-retail-sports.zip)

Baixar [Pacote de metadados do ativo](assets/we-retail-sports-asset-metadata.zip)

## Formato de arquivo de metadados {#metadata-file-format}

### Formato de arquivo CSV

#### Primeira linha

* A primeira linha do arquivo CSV define o schema de metadados.
* O padrão da primeira coluna é `assetPath`, que retém o Caminho JCR absoluto para um ativo.

* Colunas subsequentes na primeira linha apontam para outras propriedades de metadados de um ativo.

   * Por exemplo : `dc:title, dc:description, jcr:title`

* Formato de propriedade de valor único

   * `<metadata property name> {{<property type}}`
   * Se o tipo de propriedade não for especificado, o padrão será String.
   * Por exemplo: `dc:title {{String}}`

* O Nome da propriedade diferencia maiúsculas e minúsculas
   * Correto : `dc:title {{String}}`
   * Incorreto: `Dc:Title {{String}}`

* O Tipo de propriedade não diferencia maiúsculas de minúsculas
* Todos os tipos válidos de [JCR Property](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) são suportados

* Formato de propriedade de vários valores - `<metadata property name> {{<property type : MULTI }}`

#### Segunda linha para N linhas

* A primeira coluna contém o caminho JCR absoluto para um ativo. Por exemplo: /content/dam/asset1.jpg
* A propriedade de metadados de um ativo pode ter valores ausentes no arquivo CSV. A propriedade de metadados ausente para esse ativo específico não será atualizada.
