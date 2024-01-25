---
title: Utilização de importação e exportação de metadados no AEM Assets
description: Saiba como usar os recursos de importação e exportação de metadados do Adobe Experience Manager Assets. Os recursos de importação e exportação permitem que os autores de conteúdo atualizem metadados em massa para ativos existentes.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 431
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 2%

---

# Utilização de importação e exportação de metadados no AEM Assets {#metadata-import-and-export}

Saiba como usar os recursos de importação e exportação de metadados do Adobe Experience Manager Assets. Os recursos de importação e exportação permitem que os autores de conteúdo atualizem metadados em massa para ativos existentes.

## Exportação de metadados {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## Importação de metadados {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Ao preparar um arquivo CSV para importar, é mais fácil gerar um CSV com a lista de ativos usando o recurso Exportação de metadados. Você pode modificar o arquivo CSV gerado e importá-lo usando o recurso Importar.

## Formato de arquivo CSV de metadados {#metadata-file-format}

### Primeira linha

* A primeira linha do arquivo CSV define o esquema de metadados.
* O padrão da Primeira coluna é `assetPath`, que contém o Caminho absoluto do JCR de um ativo.

* As colunas subsequentes na primeira linha apontam para outras propriedades de metadados de um ativo.
   * Por exemplo: `dc:title, dc:description, jcr:title`

* Formato de Propriedade de Valor Único

   * `<metadata property name> {{<property type}}`
   * Se o tipo de propriedade não for especificado, o padrão será String.
   * Por exemplo: `dc:title {{String}}`

* O nome da propriedade diferencia maiúsculas de minúsculas
   * Correto: `dc:title {{String}}`
   * Incorreto: `Dc:Title {{String}}`

* O tipo de propriedade não diferencia maiúsculas de minúsculas
* Todos válidos [Tipos de propriedade JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) são compatíveis

* Formato de propriedade de vários valores - `<metadata property name> {{<property type : MULTI }}`

### Segunda linha para N linhas

* A primeira coluna contém o caminho absoluto do JCR de um ativo. Por exemplo: /content/dam/asset1.jpg
* A propriedade de metadados de um ativo pode ter valores ausentes no arquivo CSV. As propriedades de metadados ausentes desse ativo específico não são atualizadas.
