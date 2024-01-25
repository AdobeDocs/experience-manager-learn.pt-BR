---
title: Entender o Adobe Cloud Manager
description: O Adobe Cloud Manager fornece uma solução simples, mas eficiente, que permite gerenciamento fácil, introspecções e autoatendimento de ambientes AEM.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1026
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 14%

---

# Entender o Adobe Cloud Manager

O Adobe Cloud Manager fornece uma solução simples, mas eficiente, que permite gerenciamento fácil, introspecções e autoatendimento de ambientes AEM.

## Visão geral do Cloud Manager

Esta série de vídeos explora os principais recursos do Cloud Manager para AEM, incluindo:

* [Programas](#programs)
* [Ambientes](#environments)
* [Relatórios](#reports)
* [Pipeline de produção CI/CD](#cicd-production-pipeline)
* [Pipelines CI/CD de não produção](#cicd-non-production-pipeline)
* [Atividade](#activity)

Para obter uma visão geral completa, consulte [Guia do usuário do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/introduction.html?lang=pt-BR).

## Programas {#programs}

[Programas do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) representam conjuntos de ambientes AEM compatíveis com conjuntos lógicos de iniciativas de negócios, normalmente correspondendo a um SLA (Service Level Agreement, contrato de nível de serviço) adquirido. Por exemplo, um programa pode representar os recursos do AEM para dar suporte aos sites públicos globais, enquanto outro programa representa um DAM central interno.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Ambientes {#environments}

[Ambientes do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) são compostos por instâncias de Autor de AEM, Publicação de AEM e Dispatcher. Diferentes ambientes oferecem suporte a funções e podem ser envolvidos usando diferentes Pipelines de CI/CD (descritos abaixo). Os ambientes do Cloud Manager normalmente têm um ambiente de Produção e um ambiente de Preparo.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Relatórios {#reports}

[Relatórios do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) fornecer uma visualização dos ambientes do programa e das instâncias do AEM por meio de um conjunto de gráficos que relatam e rastreiam várias métricas para cada instância do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Pipeline de produção CI/CD {#cicd-production-pipeline}

*[Usar o pipeline de CI/CD no Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) A série de vídeos oferece um aprofundamento sobre a execução do pipeline de produção, incluindo a exploração de implantações com falha e bem-sucedidas.*

>[!NOTE]
>
> Ao longo desses vídeos, os tempos de build, teste e implantação foram acelerados para reduzir o tempo do vídeo. Uma execução completa do pipeline geralmente leva 45 minutos ou mais (incluindo o teste de desempenho obrigatório de 30 minutos), dependendo do tamanho do projeto, do número de instâncias de AEM e dos processos UAT.

### Configuração

A variável [Pipeline de produção de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) A configuração do define o acionador que inicia o pipeline e os parâmetros que controlam a implantação de produção e os parâmetros de teste de desempenho.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Execução de pipeline

A variável [Pipeline de produção de CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) O é usado para criar e implantar código por meio do ambiente de Preparo para o ambiente de Produção, reduzindo o tempo de implantação.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Pipelines CI/CD de não produção {#cicd-non-production-pipeline}

[Pipelines CI/CD de não produção](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) são divididos em duas categorias: Pipelines de qualidade do código e Pipelines de implantação. A qualidade do código canaliza todo o código de uma ramificação (branch) do Git para criar e ser avaliado em relação à verificação de qualidade do código do Cloud Manager. Os pipelines de implantação oferecem suporte à implantação automatizada do código do repositório Git para qualquer ambiente não relacionado à produção, ou seja, qualquer ambiente do AEM provisionado que não seja Estágio ou Produção.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Atividade {#activity}

O Cloud Manager fornece uma visualização consolidada da atividade de um Programa, listando todas as execuções do pipeline CI/CD, tanto de produção quanto de não produção, permitindo visibilidade sobre a atividade anterior e atual, e os detalhes de qualquer atividade podem ser revisados.

O Cloud Manager também se integra a um nível por usuário com [Notificações do Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), proporcionando uma visão onipresente dos eventos e ações de interesse.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
