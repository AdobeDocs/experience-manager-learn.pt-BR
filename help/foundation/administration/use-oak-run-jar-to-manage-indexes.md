---
title: Usar oak-run.jar para gerenciar índices
description: O comando index do oak-run.jar consolida vários recursos para gerenciar índices Oak no AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e a reindexação de índices.
version: 6.4, 6.5
feature: Search
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Usar oak-run.jar para gerenciar índices

[!DNL oak-run.jar]O comando index do consolida vários recursos para gerenciar [!DNL Oak]200 índices no AEM, desde a coleta de estatísticas de índice, execução de verificações de consistência de índice e reindexação de índices.

>[!NOTE]
>
>Neste artigo e vídeos, os termos indexação e reindexação são usados alternadamente e considerados a mesma operação.

## [!DNL oak-run.jar] Noções básicas sobre o comando index

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* A versão de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) usado deve corresponder à versão do Oak usada na instância do AEM.
* Gerenciamento de índices usando [!DNL oak-run.jar] utiliza o **[!DNL index]** comando com vários sinalizadores para suportar operações diferentes.

   * `java -jar oak-run*.jar index ...`

## Estatísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` despeja todas as definições de índice, estatísticas de índice importantes e conteúdo de índice para análise offline.
* A coleta de estatísticas de índice é segura para ser executada em instâncias AEM em uso.

## Verificação de consistência do índice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se os índices do lucene Oak estão corrompidos.
* A verificação de consistência é segura para ser executada na instância AEM em uso para os níveis de verificação de consistência 1 e 2.

## Indexação online do TarMK com [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Indexação online de [!DNL TarMK] usar [!DNL oak-run.jar] é mais rápido que a configuração `reindex=true` no `oak:queryIndexDefinition` nó. Apesar desse aumento de desempenho, a indexação online usando [!DNL oak-run.jar] ainda requer uma janela de manutenção para executar a indexação.

* Indexação online de [!DNL TarMK] usar [!DNL oak-run.jar] deve **não** ser executado em instâncias de AEM fora da janela de manutenção de instâncias de AEM.

## Indexação offline do TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Indexação offline de [!DNL TarMK] usar [!DNL oak-run.jar] é o mais simples [!DNL oak-run.jar] abordagem de indexação baseada em [!DNL TarMK] uma vez que exige uma única [!DNL oak-run.jar] , no entanto, requer que a instância AEM seja encerrada.

## Indexação fora de banda TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* Indexação fora de banda ativada [!DNL TarMK] usar [!DNL oak-run.jar] minimiza o impacto da indexação em instâncias AEM em uso.
* A indexação fora da banda é a abordagem de indexação recomendada para instalações de AEM em que o tempo de reindexação/indexação excede as janelas de manutenção disponíveis.

## Indexação online do MongoMK com oak-run.jar

* Índice online com [!DNL oak-run.jar] em [!DNL MongoMK] e [!DNL RDBMK] é o método recomendado para reindexação [!DNL MongoMK] (e [!DNL RDBMK]) Instalações de AEM. **Nenhum outro método deve ser usado para [!DNL MongoMK] ou [!DNL RDBMK].**
* Essa indexação precisa ser executada somente em uma única instância do AEM no cluster.
* Indexação online de [!DNL MongoMK] é seguro para execução em um cluster AEM em execução, pois a passagem do repositório ocorrerá em apenas um [!DNL MongoDB] permitindo que os outros continuem atendendo solicitações sem impacto significativo no desempenho.

A variável [!DNL oak-run.jar] comando index para executar uma indexação on-line de [!DNL MongoMK] é o [igual ao [!DNL TarMK] Indexação online com [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) com a diferença de que o parâmetro de armazenamento de segmentos aponta para o [!DNL MongoDB] instância que contém o armazenamento de nós.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiais de suporte

* [Download [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Verifique se a versão baixada corresponde à versão do Oak instalada no AEM conforme descrito acima*
* [Documentação do comando Apache Jackrabbit Oak oak-run.jar Index](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
