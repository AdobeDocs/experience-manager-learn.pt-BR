---
title: Usar o pipeline de CI/CD no Adobe Cloud Manager
description: O Adobe Cloud Manager oferece um pipeline de CI/CD de autoatendimento simples, mas flexível, que permite que AEM equipes de projeto implantem código de forma rápida, segura e consistente em todos os ambientes AEM hospedados no AMS. Esta série de vídeo explora a configuração e a execução do pipeline de CI/CD do Cloud Manager em cenários de falha e sucesso.
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: Architecture
role: Developer
level: Beginner
exl-id: d5d59ef5-9343-4ac2-9053-a010decdb9b6
last-substantial-update: 2022-08-15T00:00:00Z
thumbnail: cm-pipeline.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 2%

---

# Usar o pipeline de CI/CD no Adobe Cloud Manager

O Adobe Cloud Manager oferece um pipeline de CI/CD de autoatendimento simples, mas flexível, que permite que AEM equipes de projeto implantem código de forma rápida, segura e consistente em todos os ambientes AEM hospedados no AMS. Esta série de vídeo explora a configuração e a execução do pipeline de CI/CD do Cloud Manager em cenários de falha e sucesso.

## Introdução

Uma introdução resumida aos programas do Cloud Manager e do Cloud Manager.

>[!NOTE]
>
>Ao longo desses vídeos, os tempos de criação, teste e implantação foram acelerados para reduzir o tempo do vídeo. Normalmente, a execução completa do pipeline demora 45 minutos ou mais (incluindo o teste de desempenho obrigatório de 30 minutos), dependendo do tamanho do projeto, do número de instâncias de AEM e dos processos de UAT.

>[!VIDEO](https://video.tv.adobe.com/v/23082?quality=12&learn=on)

## Configuração do pipeline de CI/CD

Este vídeo explora a configuração do pipeline para o programa no Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/23083?quality=12&learn=on)

## Uma execução de pipeline com falha

Este vídeo explora a execução do pipeline de CI/CD usando o código que falha nas verificações de qualidade necessárias do Cloud Manager, usando a variável **[!DNL yellow]** ramificação de repositório.

>[!VIDEO](https://video.tv.adobe.com/v/23084?quality=12&learn=on)

## Uma execução bem-sucedida do pipeline

Este vídeo explora a execução bem-sucedida do pipeline de CI/CD usando o código que passa pelas verificações de qualidade necessárias do Cloud Manager, usando o **[!DNL master]** ramificação de repositório.

Este vídeo também toca no [!UICONTROL Atividade] no Cloud Manager, que permite a reentrada em execuções ativas ou a revisão de execuções concluídas ou com falha.

>[!VIDEO](https://video.tv.adobe.com/v/23085?quality=12&learn=on)

## Materiais de apoio

* [Guia do usuário do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/introduction.html?lang=pt-BR)
* [Fazer download da verificação de código [!DNL SonarQube] regras](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)
   * *XLSX disponível na parte inferior da seção vinculada*
* [[!DNL SonarQube] Índice de regras Java™](https://rules.sonarsource.com/java/)
