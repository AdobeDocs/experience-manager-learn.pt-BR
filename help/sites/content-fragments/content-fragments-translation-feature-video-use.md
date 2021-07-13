---
title: Suporte de tradução para Fragmentos de conteúdo AEM
description: Saiba como os Fragmentos de conteúdo podem ser localizados e traduzidos com o Adobe Experience Manager. Os ativos de mídia mista associados a um Fragmento de conteúdo também podem ser extraídos e traduzidos.
feature: Fragmentos de conteúdo, Gerenciador de vários sites
topic: Localização
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# Suporte de tradução para Fragmentos de conteúdo AEM {#translation-support-content-fragments}

Saiba como os Fragmentos de conteúdo podem ser localizados e traduzidos com o Adobe Experience Manager. Os ativos de mídia mista associados a um Fragmento de conteúdo também podem ser extraídos e traduzidos.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casos de uso da tradução do fragmento de conteúdo {#content-fragment-translation-use-cases}

Fragmentos de conteúdo são um tipo de conteúdo reconhecido que AEM extrações a serem enviadas para um serviço de tradução externo. Vários casos de uso são suportados imediatamente:

1. Um Fragmento de conteúdo pode ser selecionado diretamente no console Assets para cópia de idioma e tradução
2. Os Fragmentos de conteúdo referenciados em uma página Sites são copiados para a pasta de idioma apropriada e extraídos para tradução quando a página Sites é selecionada para cópia de idioma
3. Os ativos de mídia em linha incorporados dentro de um fragmento de conteúdo podem ser extraídos e traduzidos.
4. As coleções de ativos associadas a um fragmento de conteúdo podem ser extraídas e traduzidas

## Editor de regras de tradução {#translation-rules-editor}

O comportamento de tradução do Experience Manager pode ser atualizado usando o **Editor de regras de tradução**. Para atualizar a tradução, navegue até **Ferramentas** > **Geral** > **Configuração de Tradução** em [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

As configurações prontas fazem referência aos Fragmentos de conteúdo em `fragmentPath` com um tipo de recurso `core/wcm/components/contentfragment/v1/contentfragment`. Todos os componentes herdados do `v1/contentfragment` são reconhecidos pela configuração padrão.

![Editor de regras de tradução](assets/translation-configuration.png)
