---
title: Configurar modelos de ativos com AEM Assets e InDesign Server
description: Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões de visita, folhetos, anúncios e cartões de postagem é muito mais fácil com os Modelos de ativos quando integrados ao servidor do InDesign. A configuração do servidor de InDesigns com AEM é abordada nesta seção.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Configurar modelos de ativos com AEM Assets e InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Modelos de ativos permitem que os profissionais de marketing criem, gerenciem e entreguem ativos digitais para impressão digital. A criação de folhetos de marketing, cartões de visita, folhetos, anúncios e cartões de postagem é muito mais fácil com os Modelos de ativos quando integrados ao servidor do InDesign. A configuração do servidor de InDesigns com AEM é abordada nesta seção.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **deve** estar conectado a um servidor de InDesign em execução quando o modelo INDD é carregado. Parte do processamento inicial no arquivo INDD requer o servidor InDesign.

## Baixar avaliação do InDesign Server {#download-indesign-server-trial}

Baixar site de download de versão de avaliação do [InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## InDesign Server inicial {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
