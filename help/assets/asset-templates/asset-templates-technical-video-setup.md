---
title: Configurar modelos de ativos com o AEM Assets e o InDesign Server
description: Os Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões de visita, panfletos, anúncios e cartões de postagem é muito mais fácil com modelos de ativos quando integrado ao servidor do InDesign. A configuração do servidor do InDesign com o AEM é abordada nesta seção.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---


# Configurar modelos de ativos com AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Os Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões de visita, panfletos, anúncios e cartões de postagem é muito mais fácil com modelos de ativos quando integrado ao servidor do InDesign. A configuração do servidor do InDesign com o AEM é abordada nesta seção.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>O AEM **deve** estar conectado a um servidor do InDesign em execução quando o modelo INDD for carregado. Parte do processamento inicial no arquivo INDD requer o servidor do InDesign.

## Baixar a avaliação do InDesign Server {#download-indesign-server-trial}

Baixe [Site de download de avaliação do InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## Iniciando o InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
