---
title: Usar o pipeline de CI/CD no Adobe Cloud Manager
description: O Adobe Cloud Manager fornece um pipeline de CI/CD de autoatendimento simples, mas flexível, que permite que as equipes de projeto do AEM implantem código de forma rápida, segura e consistente em todos os ambientes AEM hospedados no AMS. Esta série de vídeo explora a configuração e a execução do pipeline de CI/CD do Cloud Manager em cenários de falha e sucesso.
sub-product: cloud-manager, foundation
feature: gasodutos, portões de qualidade
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# Usar o pipeline de CI/CD no Adobe Cloud Manager

O Adobe Cloud Manager fornece um pipeline de CI/CD de autoatendimento simples, mas flexível, que permite que as equipes de projeto do AEM implantem código de forma rápida, segura e consistente em todos os ambientes AEM hospedados no AMS. Esta série de vídeo explora a configuração e a execução do pipeline de CI/CD do Cloud Manager em cenários de falha e sucesso.

## Introdução

Uma introdução resumida aos programas do Cloud Manager e do Cloud Manager.

>[!NOTE]
>
>Ao longo desses vídeos, os tempos de criação, teste e implantação foram acelerados para reduzir o tempo do vídeo. Normalmente, a execução completa do pipeline demora 45 minutos ou mais (incluindo o teste de desempenho obrigatório de 30 minutos), dependendo do tamanho do projeto, do número de instâncias do AEM e dos processos de UAT.

>[!VIDEO](https://video.tv.adobe.com/v/23082/?quality=12&learn=on)

## Configuração do pipeline de CI/CD

Este vídeo explora a configuração do pipeline para o programa no Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/23083/?quality=12&learn=on)

## Uma execução de pipeline com falha

Este vídeo explora a execução do pipeline de CI/CD usando o código que falha nas verificações de qualidade necessárias do Cloud Manager, usando a ramificação de repositório **[!DNL yellow]**.

>[!VIDEO](https://video.tv.adobe.com/v/23084/?quality=12&learn=on)

## Uma execução bem-sucedida do pipeline

Este vídeo explora a execução bem-sucedida do pipeline de CI/CD usando o código que passa pelas verificações de qualidade necessárias do Cloud Manager, usando a ramificação de repositório **[!DNL master]**.

Este vídeo também toca no console [!UICONTROL Activity] no Cloud Manager, que permite a reentrada em execuções ativas ou a revisão de execuções concluídas ou com falha.

>[!VIDEO](https://video.tv.adobe.com/v/23085/?quality=12&learn=on)

## Materiais de apoio

* [Guia do usuário do Cloud Manager](https://helpx.adobe.com/experience-manager/cloud-manager/user-guide.html)
* [Baixar regras de  [!DNL SonarQube] verificação do código](https://helpx.adobe.com/experience-manager/cloud-manager/using/understand-your-test-results.html#CodeQualityTesting)
   * *XLSX disponível na parte inferior da seção vinculada*
* [[!DNL SonarQube] Índice de regras Java](https://rules.sonarsource.com/java/)
