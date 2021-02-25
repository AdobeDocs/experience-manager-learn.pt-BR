---
title: Suporte de tradução para AEM fragmentos de conteúdo
description: Saiba como os Fragmentos de conteúdo podem ser localizados e traduzidos com o Adobe Experience Manager. Os ativos de mídia mista associados a um Fragmento de conteúdo também podem ser extraídos e convertidos.
feature: Fragmentos de conteúdo, Gerenciador de vários sites
topics: Localization
role: Praticante de negócios
level: Intermediário
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: 4620acc18a08d71994753903b79247a8ed3fd8f5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---


# Suporte de tradução para AEM fragmentos de conteúdo {#translation-support-content-fragments}

Saiba como os Fragmentos de conteúdo podem ser localizados e traduzidos com o Adobe Experience Manager. Os ativos de mídia mista associados a um Fragmento de conteúdo também podem ser extraídos e convertidos.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casos de uso da tradução do fragmento do conteúdo {#content-fragment-translation-use-cases}

Fragmentos de conteúdo são um tipo de conteúdo reconhecido que AEM extratos para serem enviados a um serviço de tradução externo. Vários casos de uso são suportados prontamente:

1. Um Fragmento de conteúdo pode ser selecionado diretamente no console Ativos para cópia e tradução de idiomas
2. Fragmentos de conteúdo referenciados em uma página Sites são copiados para a pasta de idioma apropriada e extraídos para conversão quando a página Sites é selecionada para cópia de idioma
3. Os ativos de mídia incorporados em um fragmento de conteúdo são elegíveis para serem extraídos e convertidos.
4. As coleções de ativos associadas a um fragmento de conteúdo são elegíveis para extração e tradução

## Editor de regras de tradução {#translation-rules-editor}

O comportamento de conversão de Experience Manager pode ser atualizado usando o **Editor de regras de tradução**. Para atualizar a tradução, navegue até **Ferramentas** > **Geral** > **Configuração de Tradução** em [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

As configurações predefinidas fazem referência aos Fragmentos de conteúdo em `fragmentPath` com um tipo de recurso de `core/wcm/components/contentfragment/v1/contentfragment`. Todos os componentes herdados do `v1/contentfragment` são reconhecidos pela configuração padrão.

![Editor de regras de tradução](assets/translation-configuration.png)
