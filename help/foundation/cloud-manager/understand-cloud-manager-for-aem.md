---
title: Saiba mais sobre o Adobe Cloud Manager
description: O Adobe Cloud Manager oferece uma solução simples, mas robusta, que permite gerenciamento fácil, introspecção e autoatendimento de ambientes AEM.
sub-product: gerenciador de nuvem, fundação
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 4%

---


# Saiba mais sobre o Adobe Cloud Manager

O Adobe Cloud Manager oferece uma solução simples, mas robusta, que permite gerenciamento fácil, introspecção e autoatendimento de ambientes AEM.

## Visão geral do Cloud Manager

Esta série de vídeos explora os principais recursos do Cloud Manager para AEM, incluindo:

* [Programas](#programs)
* [Ambientes](#environments)
* [Relatórios](#reports)
* [Pipeline de produção CI/CD](#cicd-production-pipeline)
* [Gasodutos IC/CD de não produção](#cicd-non-production-pipeline)
* [Atividade](#activity)

Para obter uma visão geral completa, consulte o Guia [do usuário do](https://docs.adobe.com/content/help/pt-BR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)Cloud Manager.

## Programas {#programs}

[Os Programas](https://docs.adobe.com/content/help/pt-BR/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) do Cloud Manager representam conjuntos de ambientes AEM que suportam conjuntos lógicos de iniciativas de negócios, geralmente correspondentes a um SLA (Service Level Agreement, contrato de nível de serviço) adquirido. Por exemplo, um Programa pode representar os recursos AEM para suportar os sites públicos globais, enquanto outro Programa representa um DAM Central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Ambientes {#environments}

[Os Ambientes](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) do Gerenciador de nuvem são compostos por instâncias de autor de AEM, publicação de AEM e Dispatcher. Diferentes ambientes suportam funções e podem ser envolvidos usando diferentes Pipelines CI/CD (descrito abaixo). Os ambientes do Cloud Manager geralmente têm um ambiente de produção e um ambiente de estágio.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Relatórios {#reports}

[Os Relatórios](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) do Gerenciador de nuvem fornecem uma visualização para os Ambientes do Programa e AEM instâncias por meio de um conjunto de gráficos que relatam e rastreiam várias métricas para cada instância AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD Production Pipeline {#cicd-production-pipeline}

*[Usar o pipeline de CI/CD na série de vídeos do Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)fornece um mergulho profundo na execução do pipeline de produção, incluindo a exploração de implantações com falha e bem-sucedidas.*

>[!NOTE]
>
> Ao longo desses vídeos, os tempos de criação, teste e implantação foram acelerados para reduzir o tempo do vídeo. Uma execução completa de pipeline normalmente leva 45 minutos ou mais (incluindo o teste obrigatório de desempenho de 30 minutos), dependendo do tamanho do projeto, do número de instâncias AEM e dos processos UAT.

### Configuração

A configuração [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) define o acionador que iniciará o pipeline, parâmetros que controlam a implantação da produção e os parâmetros de teste de desempenho.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Execução do pipeline

O [CI/CD Production Pipeline](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) é usado para criar e implantar o código por meio do Stage no ambiente Production, diminuindo o tempo de implantação.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Gasodutos IC/CD de não produção {#cicd-non-production-pipeline}

[IC/CD Os gasodutos](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) de não produção são divididos em duas categorias, pipelines Code Quality e pipelines Deployment. A qualidade do código anula todos os códigos de uma ramificação Git para criar e ser avaliada em relação à verificação da qualidade do código do Cloud Manager. Os pipelines de implantação oferecem suporte à implantação automatizada de código do repositório Git para qualquer ambiente que não seja de produção, ou seja, qualquer ambiente provisionado AEM que não seja Estágio ou Produção.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Atividade {#activity}

O Cloud Manager fornece uma visualização consolidada para uma atividade, relacionando todas as execuções do pipeline de CI/CD, tanto a produção quanto a não-produção, permitindo a visibilidade da atividade atual e passada, e todos os detalhes da atividade podem ser revisados.

O Cloud Manager também se integra a um nível por usuário com as Notificações [da](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)Adobe Experience Cloud, fornecendo uma visualização onipresente em eventos e ações de interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
