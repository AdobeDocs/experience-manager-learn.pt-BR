---
title: Entender o Adobe Cloud Manager
description: O Adobe Cloud Manager oferece uma solução simples, mas robusta, que permite fácil gerenciamento, introspecção e autoatendimento de ambientes AEM.
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Arquitetura
role: Architect
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# Entender o Adobe Cloud Manager

O Adobe Cloud Manager oferece uma solução simples, mas robusta, que permite fácil gerenciamento, introspecção e autoatendimento de ambientes AEM.

## Visão geral do Cloud Manager

Esta série de vídeo explora os principais recursos do Cloud Manager para AEM, incluindo:

* [Programas](#programs)
* [Ambientes](#environments)
* [Relatórios](#reports)
* [Pipeline de produção de CI/CD](#cicd-production-pipeline)
* [Pipelines de não produção de CI/CD](#cicd-non-production-pipeline)
* [Atividade](#activity)

Para obter uma visão geral completa, consulte o [Guia do Usuário do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=pt-BR).

## Programas {#programs}

[Os ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) Programas do Cloud Manager representam conjuntos de ambientes AEM que suportam conjuntos lógicos de iniciativas de negócios, normalmente correspondendo a um Contrato de nível de serviço (SLA) adquirido. Por exemplo, um Programa pode representar os recursos AEM para suportar os sites públicos globais, enquanto outro Programa representa um DAM central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Ambientes {#environments}

[Os ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) ambientes do Cloud Manager são compostos de instâncias do AEM Author, AEM Publish e Dispatcher. Ambientes diferentes oferecem suporte a funções e podem ser envolvidos usando diferentes Pipelines de CI/CD (descrito abaixo). Os ambientes do Cloud Manager normalmente têm um ambiente de Produção e um ambiente de Preparo.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Relatórios {#reports}

[Os ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) Relatórios do Cloud Manager fornecem uma visualização dos Ambientes e instâncias AEM do Programa por meio de um conjunto de gráficos que informam e rastreiam várias métricas para cada instância AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Pipeline de produção de CI/CD {#cicd-production-pipeline}

*[Usar o pipeline de CI/CD na série Adobe Cloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Managervideo fornece um mergulho profundo na execução do pipeline de produção, incluindo exploração de implantações com falha e bem-sucedidas.*

>[!NOTE]
>
> Ao longo desses vídeos, os tempos de criação, teste e implantação foram acelerados para reduzir o tempo do vídeo. Normalmente, a execução completa do pipeline demora 45 minutos ou mais (incluindo o teste de desempenho obrigatório de 30 minutos), dependendo do tamanho do projeto, do número de instâncias de AEM e dos processos de UAT.

### Configuração

A configuração [CI/CD Production Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) define o acionador que iniciará o pipeline, parâmetros que controlam a implantação de produção e parâmetros de teste de desempenho.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Execução de pipeline

O [CI/CD Production Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) é usado para criar e implantar código por meio do Stage no ambiente de Produção, diminuindo o tempo para o valor.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Pipelines de não produção de CI/CD {#cicd-non-production-pipeline}

[CI/CD Os ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) pipelines de não produção são divididos em duas categorias, pipelines de qualidade do código e pipelines de implantação. O código de pipelines de qualidade todos os códigos de uma ramificação Git para criar e ser avaliado em relação à verificação de qualidade do código do Cloud Manager. Os pipelines de implantação oferecem suporte à implantação automatizada do código do repositório Git para qualquer ambiente não relacionado à produção, ou seja, qualquer ambiente AEM provisionado que não seja Estágio ou Produção.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Atividade {#activity}

O Cloud Manager fornece uma visualização consolidada da atividade de um Programa, listando todas as execuções de pipeline de CI/CD, tanto a produção quanto a não produção, permitindo a visibilidade da atividade anterior e atual, e os detalhes de qualquer atividade podem ser revisados.

O Cloud Manager também se integra a um nível por usuário com as [Notificações do Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/notifications.html), fornecendo uma visualização uniforme em eventos e ações de interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
