---
title: Configurar modelos de ativos com AEM Assets e InDesign Server
description: Os Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões comerciais, panfletos, anúncios e cartões de postagem é muito mais fácil com Modelos de ativos quando integrado ao servidor do InDesign. A configuração do servidor InDesign com AEM é abordada nesta seção.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: 6dd7164f5ec045b4cffd7732fd83ad9a91fdd511
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Configurar modelos de ativos com AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Os Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões comerciais, panfletos, anúncios e cartões de postagem é muito mais fácil com Modelos de ativos quando integrado ao servidor do InDesign. A configuração do servidor InDesign com AEM é abordada nesta seção.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **must** ser conectado a um servidor do InDesign em execução quando o modelo INDD for carregado. Parte do processamento inicial no arquivo INDD requer o servidor InDesign.

## Baixar avaliação do InDesign Server {#download-indesign-server-trial}

Baixar [Site de download de avaliação do InDesign Server](https://www.adobeprerelease.com/)

## Iniciando o InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
