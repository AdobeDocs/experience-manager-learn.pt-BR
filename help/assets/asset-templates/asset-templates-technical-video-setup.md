---
title: Configurar modelos de ativos com o AEM Assets e o InDesign Server
description: Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão e digitais. Criar folhetos de marketing, cartões de visita, folhetos, anúncios e cartões postais é muito mais fácil com os Modelos de ativos quando integrados ao servidor do InDesign. A configuração do servidor do InDesign com AEM é abordada nesta seção.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Configurar modelos de ativos com o AEM Assets e o InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão e digitais. Criar folhetos de marketing, cartões de visita, folhetos, anúncios e cartões postais é muito mais fácil com os Modelos de ativos quando integrados ao servidor do InDesign. A configuração do servidor do InDesign com AEM é abordada nesta seção.

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>O AEM **deve** estar conectado a um servidor InDesign em execução quando o modelo INDD for carregado. Parte do processamento inicial no arquivo INDD requer o servidor do InDesign.

## Baixar avaliação do InDesign Server {#download-indesign-server-trial}

Baixe o [site de avaliação do InDesign Server](https://www.adobeprerelease.com/)

## Início do InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
