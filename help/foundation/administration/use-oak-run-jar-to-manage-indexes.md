---
title: Use oak-run.jar para gerenciar índices
description: o comando de índice oak-run.jar consolida vários recursos para gerenciar índices Oak em AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e a reindexação de índices.
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---


# Use oak-run.jar para gerenciar índices

[!DNL oak-run.jar]O comando de índice do consolida vários recursos para gerenciar [!DNL Oak]200 índices em AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e a reindexação de índices.

>[!NOTE]
>
>Neste artigo e vídeos, os termos indexação e reindexação são usados intercambiavelmente e considerados a mesma operação.

## [!DNL oak-run.jar] fundamentos do Comando de Índice

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* A versão de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) usada deve corresponder à versão do Oak usada na instância AEM.
* O gerenciamento de índices usando [!DNL oak-run.jar] potencializa o **[!DNL index]** comando com vários sinalizadores para suportar diferentes operações.

   * `java -jar oak-run*.jar index ...`

## Estatísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` descarta todas as definições de índice, estatísticas de índice importantes e conteúdo de índice para a análise offline.
* A coleta de estatísticas de índice é segura para execução em instâncias AEM em uso.

## Verificação de consistência do índice

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se os índices do lucene Oak estão corrompidos.
* A verificação de consistência é segura para ser executada na instância AEM em uso para verificar a consistência dos níveis 1 e 2.

## Indexação TarMK Online com [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* A indexação online do [!DNL TarMK] uso [!DNL oak-run.jar] é mais rápida do que a configuração `reindex=true` no `oak:queryIndexDefinition` nó. Apesar desse aumento no desempenho, a indexação on-line usando [!DNL oak-run.jar] ainda requer uma janela de manutenção para executar a indexação.

* A indexação online do [!DNL TarMK] uso [!DNL oak-run.jar] não **** deve ser executada em relação a instâncias AEM fora da janela de manutenção de instâncias AEM.

## Indexação offline TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* A indexação offline do [!DNL TarMK] uso [!DNL oak-run.jar] é a abordagem de indexação mais simples e baseada [!DNL oak-run.jar] para [!DNL TarMK] a qual requer um único [!DNL oak-run.jar] comando, no entanto, requer que a instância AEM seja desligada.

## Indexação fora de banda TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* A indexação fora de banda ao [!DNL TarMK] usar [!DNL oak-run.jar] minimiza o impacto da indexação em instâncias AEM em uso.
* A indexação fora de banda é a abordagem de indexação recomendada para instalações de AEM em que o tempo de indexação/reindexação excede as janelas de manutenção disponíveis.

## Indexação on-line MongoMK com oak-run.jar

* Índice on-line com [!DNL oak-run.jar] ativado [!DNL MongoMK] e [!DNL RDBMK] é o método recomendado para reindexar [!DNL MongoMK] (e [!DNL RDBMK]) instalações AEM. **Nenhum outro método deve ser usado para[!DNL MongoMK]ou[!DNL RDBMK].**
* Essa indexação precisa ser executada somente em uma única instância AEM no cluster.
* A indexação on-line de [!DNL MongoMK] é segura para executar em um cluster AEM em execução, já que a passagem do repositório ocorrerá em apenas um único [!DNL MongoDB] nó, permitindo que os outros continuem atendendo solicitações sem impacto significativo no desempenho.

O comando de [!DNL oak-run.jar] índice para executar uma indexação on-line do [!DNL MongoMK] é o [mesmo que [!DNL TarMK] a indexação On-line com [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) a diferença que o parâmetro de armazenamento do segmento aponta para a [!DNL MongoDB] instância que contém o armazenamento de nós.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiais de suporte

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Verifique se a versão baixada corresponde à versão do Oak instalada no AEM, conforme descrito acima*
* [Documentação do Comando do Índice Apache Jackrabbit Oak-run.jar](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
