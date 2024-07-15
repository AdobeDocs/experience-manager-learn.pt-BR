---
title: Usar oak-run.jar para gerenciar índices
description: O comando index do oak-run.jar consolida vários recursos para gerenciar índices Oak no AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e a reindexação de índices.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Usar oak-run.jar para gerenciar índices

O comando index de [!DNL oak-run.jar] consolida vários recursos para gerenciar [!DNL Oak]200 índices no AEM, desde a coleta de estatísticas de índice, a execução de verificações de consistência de índice e a reindexação de índices.

>[!NOTE]
>
>Neste artigo e vídeos, os termos indexação e reindexação são usados alternadamente e considerados a mesma operação.

## Fundamentos do comando de índice [!DNL oak-run.jar]

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* A versão de [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) usada deve corresponder à versão do Oak usada na instância AEM.
* O gerenciamento de índices usando [!DNL oak-run.jar] aproveita o comando **[!DNL index]** com vários sinalizadores para dar suporte a diferentes operações.

   * `java -jar oak-run*.jar index ...`

## Estatísticas de índice

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` descarta todas as definições de índice, estatísticas de índice importantes e conteúdo de índice para análise offline.
* A coleta de estatísticas de índice é segura para ser executada em instâncias AEM em uso.

## Verificação de consistência do índice

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` determina rapidamente se os índices Lucene Oak estão corrompidos.
* A verificação de consistência é segura para ser executada na instância AEM em uso para os níveis de verificação de consistência 1 e 2.

## Indexação TarMK Online com [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* A indexação online de [!DNL TarMK] usando [!DNL oak-run.jar] é mais rápida que a configuração `reindex=true` no nó `oak:queryIndexDefinition`. Apesar desse aumento de desempenho, a indexação online usando o [!DNL oak-run.jar] ainda requer uma janela de manutenção para executar a indexação.

* A indexação online de [!DNL TarMK] usando [!DNL oak-run.jar] deve **não** ser executada em instâncias AEM fora da janela de manutenção de instâncias AEM.

## Indexação offline do TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* A indexação offline de [!DNL TarMK] usando [!DNL oak-run.jar] é a abordagem de indexação baseada em [!DNL oak-run.jar] mais simples para [!DNL TarMK], pois requer um único comando [!DNL oak-run.jar], no entanto, requer que a instância de AEM seja desligada.

## Indexação fora de banda TarMK com oak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* A indexação fora de banda em [!DNL TarMK] usando [!DNL oak-run.jar] minimiza o impacto da indexação em instâncias AEM em uso.
* A indexação fora da banda é a abordagem de indexação recomendada para instalações de AEM em que o tempo de reindexação/indexação excede as janelas de manutenção disponíveis.

## Indexação online do MongoMK com oak-run.jar

* O índice online com [!DNL oak-run.jar] em [!DNL MongoMK] e [!DNL RDBMK] é o método recomendado para reindexar [!DNL MongoMK] (e [!DNL RDBMK]) instalações de AEM. **Nenhum outro método deve ser usado para [!DNL MongoMK] ou [!DNL RDBMK].**
* Essa indexação precisa ser executada somente em uma única instância do AEM no cluster.
* A indexação online de [!DNL MongoMK] é segura para execução em um cluster AEM em execução, pois a passagem do repositório ocorrerá em apenas um único nó [!DNL MongoDB], permitindo que os outros continuem atendendo solicitações sem impacto significativo no desempenho.

O comando index [!DNL oak-run.jar] para executar uma indexação online de [!DNL MongoMK] é o [mesmo que o [!DNL TarMK] indexação Online com [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) com a diferença de que o parâmetro do repositório de segmentos aponta para a instância [!DNL MongoDB] que contém o repositório de nós.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Materiais de suporte

* [Baixar [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Verifique se a versão baixada corresponde à versão do Oak instalada no AEM conforme descrito acima*
* [Documentação de comando do Apache Jackrabbit Oak oak-run.jar Index](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
