---
title: Usar o pipeline CI/CD no Gerenciador da Adobe Cloud
description: O Adobe Cloud Manager fornece um pipeline de IC/CD de autoatendimento simples, mas flexível, que permite que AEM equipes de projeto implantem código de forma rápida, segura e consistente em todos os ambientes AEM hospedados no AMS. Esta série de vídeos explora a configuração e a execução do pipeline CI/CD do Cloud Manager em cenários de falha e sucesso.
sub-product: gerenciador de nuvem, fundação
feature: pipelines, quality-gates
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---


# Usar o pipeline CI/CD no Gerenciador da Adobe Cloud

O Adobe Cloud Manager fornece um pipeline de IC/CD de autoatendimento simples, mas flexível, que permite que AEM equipes de projeto implantem código de forma rápida, segura e consistente em todos os ambientes AEM hospedados no AMS. Esta série de vídeos explora a configuração e a execução do pipeline CI/CD do Cloud Manager em cenários de falha e sucesso.

## Introdução

Uma breve introdução aos Programas do Cloud Manager e do Cloud Manager.

>[!NOTE]
>
>Ao longo desses vídeos, os tempos de criação, teste e implantação foram acelerados para reduzir o tempo do vídeo. Uma execução completa de pipeline normalmente leva 45 minutos ou mais (incluindo o teste obrigatório de desempenho de 30 minutos), dependendo do tamanho do projeto, do número de instâncias AEM e dos processos UAT.

>[!VIDEO](https://video.tv.adobe.com/v/23082/?quality=12&learn=on)

## Configuração do pipeline de CI/CD

Este vídeo explora a configuração do Pipeline para o Programa no Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/23083/?quality=12&learn=on)

## Uma execução de pipeline com falha

Este vídeo explora a execução do Pipeline de CI/CD usando o código que falha nas verificações de qualidade necessárias do Cloud Manager, usando a ramificação do **[!DNL yellow]** repositório.

>[!VIDEO](https://video.tv.adobe.com/v/23084/?quality=12&learn=on)

## Uma execução bem-sucedida do pipeline

Este vídeo explora a execução bem-sucedida do CI/CD Pipeline usando o código que passa pelas verificações de qualidade necessárias do Cloud Manager, usando a ramificação do **[!DNL master]** repositório.

Este vídeo também toca no console de [!UICONTROL Atividade] no Cloud Manager, que permite a reentrada em execuções ativas ou a revisão de execuções concluídas ou com falha.

>[!VIDEO](https://video.tv.adobe.com/v/23085/?quality=12&learn=on)

## Materiais de suporte

* [Guia do usuário do Cloud Manager](https://helpx.adobe.com/experience-manager/cloud-manager/user-guide.html)
* [Baixar regras de [!DNL SonarQube] verificação de código](https://helpx.adobe.com/experience-manager/cloud-manager/using/understand-your-test-results.html#CodeQualityTesting)
   * *XLSX disponível na parte inferior da seção vinculada*
* [[!DNL SonarQube] Índice de regras Java](https://rules.sonarsource.com/java/)
