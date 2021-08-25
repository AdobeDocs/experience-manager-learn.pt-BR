---
title: Configurar modelos de ativos com AEM Assets e InDesign Server
description: Os Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões comerciais, panfletos, anúncios e cartões de postagem é muito mais fácil com Modelos de ativos quando integrado ao servidor do InDesign. A configuração do servidor InDesign com AEM é abordada nesta seção.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# Configurar modelos de ativos com AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Os Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões comerciais, panfletos, anúncios e cartões de postagem é muito mais fácil com Modelos de ativos quando integrado ao servidor do InDesign. A configuração do servidor InDesign com AEM é abordada nesta seção.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **deve** ser conectado a um servidor do InDesign em execução quando o modelo INDD for carregado. Parte do processamento inicial no arquivo INDD requer o servidor InDesign.

## Baixar avaliação do InDesign Server {#download-indesign-server-trial}

Baixar [Download da avaliação do InDesign Server Site](https://www.adobe.com/devnet/premiere/sdk/cs5/indesign-server-trial-downloads.html)

## Iniciando o InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
