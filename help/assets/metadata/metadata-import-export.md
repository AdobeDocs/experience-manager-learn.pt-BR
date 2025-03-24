---
title: Utilização de importação e exportação de metadados no AEM Assets
description: Saiba como usar os recursos de importação e exportação de metadados do Adobe Experience Manager Assets. Os recursos de importação e exportação permitem que os autores de conteúdo atualizem metadados em massa para ativos existentes.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# Utilização de importação e exportação de metadados no AEM Assets {#metadata-import-and-export}

Saiba como usar os recursos de importação e exportação de metadados do Adobe Experience Manager Assets. Os recursos de importação e exportação permitem que os autores de conteúdo atualizem metadados em massa para ativos existentes.

## Exportação de metadados {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> Ao abrir o arquivo CSV de exportação de metadados no Excel, use o [importador do Excel](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6) em vez de clicar duas vezes no arquivo para evitar problemas com arquivos CSV codificados em UTF-8.
>
> Para abrir o arquivo CSV de exportação de metadados no Excel, siga estas etapas:
> 
> 1. Abrir o Microsoft Excel
> 1. Selecione __Arquivo > Novo__ para criar uma planilha vazia
> 1. Com a planilha vazia aberta, selecione __Arquivo > Importar__
> 1. Selecione o arquivo __Texto__ e clique em __Importar__
> 1. Selecione o arquivo CSV exportado do sistema de arquivos e clique em __Obter dados__
> 1. Na etapa 1 do assistente de importação, selecione __Delimitado__ e defina __Origem do arquivo__ como __Unicode (UTF-8)__ e clique em __Avançar__
> 1. Na etapa 2, defina os __Delimitadores__ como __Vírgula__ e clique em __Avançar__
> 1. Na etapa 3, deixe o __Formato de dados da coluna__ como está e clique em __Concluir__
> 1. Selecione __Importar__ para adicionar os dados à planilha

## Importação de metadados {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Ao preparar um arquivo CSV para importar, é mais fácil gerar um CSV com a lista de ativos usando o recurso Exportação de metadados. Você pode modificar o arquivo CSV gerado e importá-lo usando o recurso Importar.

## Formato de arquivo CSV de metadados {#metadata-file-format}

### Primeira linha

* A primeira linha do arquivo CSV define o esquema de metadados.
* O padrão da Primeira coluna é `assetPath`, que contém o Caminho JCR absoluto para um ativo.

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
* Todos os [tipos de propriedades JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) válidos são suportados

* Formato da Propriedade de Vários Valores - `<metadata property name> {{<property type : MULTI }}`

### Segunda linha para N linhas

* A primeira coluna contém o caminho absoluto do JCR de um ativo. Por exemplo: /content/dam/asset1.jpg
* A propriedade de metadados de um ativo pode ter valores ausentes no arquivo CSV. As propriedades de metadados ausentes desse ativo específico não são atualizadas.
