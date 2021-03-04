---
title: Usar o oak-run.jar para gerenciar índices
description: o comando index do oak-run.jar consolida vários recursos para gerenciar índices do Oak no AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e os próprios índices de reindexação.
version: 6.4, 6.5
feature: carvalho
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# Usar o oak-run.jar para gerenciar índices

[!DNL oak-run.jar]O comando de índice do consolida vários recursos para gerenciar  [!DNL Oak]200 índices no AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e os próprios índices de reindexação.

>[!NOTE]
>
>Neste artigo e vídeos, os termos indexação e reindexação são usados alternadamente e considerados a mesma operação.

## [!DNL oak-run.jar] Noções básicas sobre o comando index

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* A versão de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) usada deve corresponder à versão do Oak usada na instância do AEM.
* O gerenciamento de índices usando [!DNL oak-run.jar] aproveita o comando **[!DNL index]** com vários sinalizadores para suportar operações diferentes.

   * `java -jar oak-run*.jar index ...`

## Estatísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` despeja todas as definições de índice, estatísticas de índice importantes e conteúdo de índice para análise offline.
* A coleta de estatísticas de índice é segura para executar em instâncias do AEM em uso.

## Verificação de consistência do índice

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se os índices do lucene Oak estão corrompidos.
* A verificação de consistência é segura para ser executada na instância do AEM em uso para verificar a consistência nos níveis 1 e 2.

## Indexação TarMK Online com [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* A indexação online de [!DNL TarMK] usando [!DNL oak-run.jar] é mais rápida que definir `reindex=true` no nó `oak:queryIndexDefinition`. Apesar desse aumento de desempenho, a indexação online usando [!DNL oak-run.jar] ainda requer uma janela de manutenção para executar a indexação.

* A indexação online de [!DNL TarMK] usando [!DNL oak-run.jar] deve **não** ser executada em instâncias do AEM fora da janela de manutenção de instâncias do AEM.

## Indexação offline TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* A indexação offline de [!DNL TarMK] usando [!DNL oak-run.jar] é a abordagem de indexação baseada em [!DNL oak-run.jar] mais simples para [!DNL TarMK], pois requer um único comando [!DNL oak-run.jar], no entanto, requer que a instância do AEM seja desligada.

## Indexação fora de banda TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* A indexação fora de banda em [!DNL TarMK] usando [!DNL oak-run.jar] minimiza o impacto da indexação nas instâncias do AEM em uso.
* A indexação fora de banda é a abordagem de indexação recomendada para instalações do AEM em que o tempo para reindexar/indexar excede as janelas de manutenção disponíveis.

## Indexação online do MongoMK com oak-run.jar

* O índice online com [!DNL oak-run.jar] em [!DNL MongoMK] e [!DNL RDBMK] é o método recomendado para reindexar [!DNL MongoMK] (e [!DNL RDBMK]) instalações do AEM. **Nenhum outro método deve ser usado para  [!DNL MongoMK] ou  [!DNL RDBMK].**
* Essa indexação precisa ser executada somente em uma única instância do AEM no cluster.
* A indexação online de [!DNL MongoMK] é segura para ser executada em um cluster AEM em execução, pois a passagem do repositório ocorrerá em apenas um único nó [!DNL MongoDB], permitindo que os outros continuem atendendo solicitações sem impacto significativo no desempenho.

O comando de índice [!DNL oak-run.jar] para executar uma indexação online de [!DNL MongoMK] é o [mesmo que a [!DNL TarMK] Indexação online com [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) com a diferença de que o parâmetro de armazenamento de segmento aponta para a instância [!DNL MongoDB] que contém o armazenamento de nós.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiais de apoio

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Verifique se a versão baixada corresponde à versão do Oak instalada no AEM, conforme descrito acima*
* [Documentação de comando do índice Apache Jackrabbit Oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
