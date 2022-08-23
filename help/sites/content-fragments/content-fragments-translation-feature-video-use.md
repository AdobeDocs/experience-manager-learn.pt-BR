---
title: Suporte de tradução para Fragmentos de conteúdo AEM
description: Saiba como os Fragmentos de conteúdo podem ser localizados e traduzidos com o Adobe Experience Manager. Os ativos de mídia mista associados a um Fragmento de conteúdo também podem ser extraídos e traduzidos.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---

# Suporte de tradução para Fragmentos de conteúdo AEM {#translation-support-content-fragments}

Saiba como os Fragmentos de conteúdo podem ser localizados e traduzidos com o Adobe Experience Manager. Os ativos de mídia mista associados a um Fragmento de conteúdo também podem ser extraídos e traduzidos.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casos de uso da tradução do fragmento de conteúdo {#content-fragment-translation-use-cases}

Fragmentos de conteúdo são um tipo de conteúdo reconhecido que AEM extrações a serem enviadas para um serviço de tradução externo. Vários casos de uso são suportados imediatamente:

1. Um Fragmento de conteúdo pode ser [selecionado diretamente no console Assets para cópia e tradução de idioma](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).
2. Os Fragmentos de conteúdo referenciados em uma página Sites são copiados para a pasta de idioma apropriada e extraídos para tradução quando a página Sites é selecionada para cópia de idioma.
3. Os ativos de mídia em linha incorporados dentro de um fragmento de conteúdo podem ser extraídos e traduzidos.
4. As coleções de ativos associadas a um fragmento de conteúdo podem ser extraídas e traduzidas.

## Editor de regras de tradução {#translation-rules-editor}

O comportamento de tradução do Experience Manager pode ser atualizado usando o **Editor de regras de tradução**. Para atualizar a tradução, navegue até **Ferramentas** > **Geral** > **Configuração de tradução** at [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

As configurações prontas fazem referência aos Fragmentos de conteúdo em `fragmentPath` com um tipo de recurso de `core/wcm/components/contentfragment/v1/contentfragment`. Todos os componentes herdados do `v1/contentfragment` são reconhecidas pela configuração padrão.

![Editor de regras de tradução](assets/translation-configuration.png)
