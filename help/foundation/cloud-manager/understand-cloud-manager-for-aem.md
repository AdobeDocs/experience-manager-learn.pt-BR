---
title: Entender o Adobe Cloud Manager
description: O Adobe Cloud Manager oferece uma solução simples, mas robusta, que permite gerenciamento fácil, introspectos e autoatendimento de ambientes AEM.
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 1%

---

# Entender o Adobe Cloud Manager

O Adobe Cloud Manager oferece uma solução simples, mas robusta, que permite gerenciamento fácil, introspectos e autoatendimento de ambientes AEM.

## Visão geral do Cloud Manager

Esta série de vídeo explora os principais recursos do Cloud Manager para AEM, incluindo:

* [Programas](#programs)
* [Ambientes](#environments)
* [Relatórios](#reports)
* [Pipeline de produção de CI/CD](#cicd-production-pipeline)
* [Pipelines de não produção de CI/CD](#cicd-non-production-pipeline)
* [Atividade](#activity)

Para obter uma visão geral completa, consulte o [Guia do usuário do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## Programas {#programs}

[Programas do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) representam conjuntos de ambientes AEM que oferecem suporte a conjuntos lógicos de iniciativas de negócios, normalmente correspondentes a um Contrato de nível de serviço (SLA) adquirido. Por exemplo, um Programa pode representar os recursos AEM para suportar os sites públicos globais, enquanto outro Programa representa um DAM central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Ambientes {#environments}

[Ambientes do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) são compostos de instâncias de Autor do AEM, Publicação do AEM e Dispatcher. Ambientes diferentes oferecem suporte a funções e podem ser envolvidos usando diferentes Pipelines de CI/CD (descrito abaixo). Os ambientes do Cloud Manager normalmente têm um ambiente de Produção e um ambiente de Preparo.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Relatórios {#reports}

[Relatórios do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) forneça uma visualização dos Ambientes do Programa e das instâncias de AEM por meio de um conjunto de gráficos que relatam e rastreiam várias métricas para cada instância de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Pipeline de produção de CI/CD {#cicd-production-pipeline}

*[Usar o pipeline de CI/CD no Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) a série de vídeos fornece um mergulho profundo na execução do pipeline de produção, incluindo exploração de implantações com falha e bem-sucedidas.*

>[!NOTE]
>
> Ao longo desses vídeos, os tempos de criação, teste e implantação foram acelerados para reduzir o tempo do vídeo. Normalmente, a execução completa do pipeline demora 45 minutos ou mais (incluindo o teste de desempenho obrigatório de 30 minutos), dependendo do tamanho do projeto, do número de instâncias de AEM e dos processos de UAT.

### Configuração

O [Pipeline de produção de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) A configuração define o acionador que inicia o pipeline e os parâmetros que controlam a implantação de produção e os parâmetros de teste de desempenho.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Execução de pipeline

O [Pipeline de produção de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) O é usado para criar e implantar código por meio do Stage para o ambiente de produção, diminuindo o tempo para valor.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Pipelines de não produção de CI/CD {#cicd-non-production-pipeline}

[Gasodutos de não produção CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) são divididas em duas categorias: pipelines de qualidade de código e pipelines de implantação. O código de pipelines de qualidade todos os códigos de uma ramificação Git para criar e ser avaliado em relação à verificação de qualidade do código do Cloud Manager. Os pipelines de implantação oferecem suporte à implantação automatizada do código do repositório Git para qualquer ambiente não relacionado à produção, ou seja, qualquer ambiente AEM provisionado que não seja Estágio ou Produção.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Atividade {#activity}

O Cloud Manager fornece uma visualização consolidada da atividade de um Programa, listando todas as execuções de pipeline de CI/CD, tanto a produção quanto a não produção, permitindo a visibilidade da atividade anterior e atual, e os detalhes de qualquer atividade podem ser revisados.

O Cloud Manager também se integra a um nível por usuário com [Notificações da Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), fornecendo uma visão coerente dos acontecimentos e das ações de interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
