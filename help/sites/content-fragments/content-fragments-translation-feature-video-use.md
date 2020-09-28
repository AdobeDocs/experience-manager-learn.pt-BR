---
title: Uso da tradução com AEM fragmentos de conteúdo
description: AEM 6.3 apresenta a capacidade de traduzir Fragmentos de conteúdo. Os ativos de mídia mista e as coleções de ativos associados a um Fragmento de conteúdo também são elegíveis para serem extraídos e convertidos.
sub-product: sites, ativos, serviços de conteúdo
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# Uso da tradução com AEM fragmentos de conteúdo{#using-translation-with-aem-content-fragments}

AEM 6.3 apresenta a capacidade de traduzir Fragmentos de conteúdo. Os ativos de mídia mista e as coleções de ativos associados a um Fragmento de conteúdo também são elegíveis para serem extraídos e convertidos.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## Casos de uso da tradução do fragmento do conteúdo {#content-fragment-translation-use-cases}

Fragmentos de conteúdo são um tipo de conteúdo reconhecido que AEM será extraído para ser enviado a um serviço de tradução externo. Vários casos de uso são suportados prontamente:

1. Um Fragmento de conteúdo pode ser selecionado diretamente no console Ativos para cópia e tradução de idiomas
2. Fragmentos de conteúdo referenciados em uma página Sites serão copiados para a pasta de idioma apropriada e extraídos para conversão quando a página Sites for selecionada para cópia de idioma
3. Os ativos de mídia incorporados em um fragmento de conteúdo são elegíveis para serem extraídos e convertidos.
4. As coleções de ativos associadas a um fragmento de conteúdo são elegíveis para extração e tradução

## Opções de configuração de tradução {#translation-config-options}

A configuração de tradução imediata suporta várias opções para traduzir Fragmentos de conteúdo. Por padrão, os ativos de mídia em linha e as coleções de ativos associados NÃO são convertidos. Para atualizar a configuração de conversão, navegue até [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html).

Há quatro opções para traduzir ativos de Fragmento de conteúdo:

1. **Não traduzir (padrão)**
2. **Somente ativos de mídia integrados**
3. **Somente coleções de ativos associadas**
4. **Ativos de mídia integrados e coleções associadas**

![Configurações de tradução](assets/classic-ui-dialog.png)
