---
title: Uso de importação e exportação de metadados no AEM Assets
description: Saiba como usar os recursos de metadados de importação e exportação do Adobe Experience Manager Assets. Os recursos de importação e exportação permitem que os autores de conteúdo atualizem metadados em massa para ativos existentes.
version: 6.3, 6.4, 6.5, cloud-service
topic: Gerenciamento de conteúdo
feature: Metadados
role: Admin
level: Intermediário
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# Uso de importação e exportação de metadados no AEM Assets {#metadata-import-and-export}

Saiba como usar os recursos de metadados de importação e exportação do Adobe Experience Manager Assets. Os recursos de importação e exportação permitem que os autores de conteúdo atualizem metadados em massa para ativos existentes.

## Exportação de metadados {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Importação de metadados {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Ao preparar um arquivo CSV para importação, é mais fácil gerar um CSV com a lista de ativos usando o recurso Exportação de metadados. Em seguida, você pode modificar o arquivo CSV gerado e importá-lo usando o recurso Importar .

## Formato de arquivo CSV de metadados {#metadata-file-format}

### Primeira linha

* A primeira linha do arquivo CSV define o esquema de metadados.
* O padrão da primeira coluna é `assetPath`, que retém o Caminho JCR absoluto para um ativo.

* Colunas subsequentes na primeira linha apontam para outras propriedades de metadados de um ativo.
   * Por exemplo : `dc:title, dc:description, jcr:title`

* Formato de propriedade de valor único

   * `<metadata property name> {{<property type}}`
   * Se o tipo de propriedade não for especificado, o padrão será String.
   * Por exemplo: `dc:title {{String}}`

* O nome da propriedade diferencia maiúsculas de minúsculas
   * Correto : `dc:title {{String}}`
   * Incorreto: `Dc:Title {{String}}`

* O tipo de propriedade não diferencia maiúsculas de minúsculas
* Todos os tipos válidos de [Propriedade JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) são suportados

* Formato de propriedade de vários valores - `<metadata property name> {{<property type : MULTI }}`

### Segunda linha para N linhas

* A primeira coluna contém o caminho JCR absoluto de um ativo. Por exemplo: /content/dam/asset1.jpg
* A propriedade de metadados de um ativo pode ter valores ausentes no arquivo CSV. As propriedades de metadados ausentes para esse ativo específico não são atualizadas.
