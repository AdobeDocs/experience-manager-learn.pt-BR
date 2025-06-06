---
title: Usar o pipeline de CI/CD no Adobe Cloud Manager
description: O Adobe Cloud Manager fornece um pipeline de CI/CD de autoatendimento simples, mas flexível, que permite que as equipes de projeto do AEM AEM implantem código de forma rápida, segura e consistente em todos os ambientes de hospedados no AMS. Esta série de vídeos explora a configuração e a execução do pipeline de CI/CD do Cloud Manager em cenários de falha e sucesso.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Developer
level: Beginner
exl-id: d5d59ef5-9343-4ac2-9053-a010decdb9b6
last-substantial-update: 2022-08-15T00:00:00Z
thumbnail: cm-pipeline.jpg
duration: 619
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 0%

---

# Usar o pipeline de CI/CD no Adobe Cloud Manager

O Adobe Cloud Manager fornece um pipeline de CI/CD de autoatendimento simples, mas flexível, que permite que as equipes de projeto do AEM AEM implantem código de forma rápida, segura e consistente em todos os ambientes de hospedados no AMS. Esta série de vídeos explora a configuração e a execução do pipeline de CI/CD do Cloud Manager em cenários de falha e sucesso.

## Introdução

Uma breve introdução aos programas do Cloud Manager e do Cloud Manager.

>[!NOTE]
>
>Ao longo desses vídeos, os tempos de build, teste e implantação foram acelerados para reduzir o tempo do vídeo. Uma execução completa do pipeline geralmente leva 45 minutos ou mais (incluindo o teste de desempenho obrigatório de 30 minutos), dependendo do tamanho do projeto, do número de instâncias de AEM e dos processos UAT.

>[!VIDEO](https://video.tv.adobe.com/v/34879?quality=12&learn=on&captions=por_br)

## Configuração do pipeline de CI/CD

Este vídeo explora a configuração do pipeline para o programa no Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/34880?quality=12&learn=on&captions=por_br)

## Uma execução de pipeline com falha

Este vídeo explora a execução do pipeline de CI/CD usando um código que falha nas verificações de qualidade necessárias da Cloud Manager, usando a ramificação de repositório **[!DNL yellow]**.

>[!VIDEO](https://video.tv.adobe.com/v/34881?quality=12&learn=on&captions=por_br)

## Uma execução de pipeline bem-sucedida

Este vídeo explora a execução bem-sucedida do pipeline de CI/CD usando o código que passa nas verificações de qualidade necessárias da Cloud Manager, usando a ramificação de repositório **[!DNL master]**.

Este vídeo também aborda o console [!UICONTROL Atividade] no Cloud Manager, que permite a reentrada em execuções ativas ou a revisão de execuções concluídas ou com falha.

>[!VIDEO](https://video.tv.adobe.com/v/34882?quality=12&learn=on&captions=por_br)

## Materiais de suporte

* [Guia do Usuário do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/introduction.html?lang=pt-BR)
* [Verificação de código de download [!DNL SonarQube] regras](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=pt-BR)
   * *XLSX disponível na parte inferior da seção vinculada*
* [[!DNL SonarQube] Índice de regras Java™](https://rules.sonarsource.com/java/)
