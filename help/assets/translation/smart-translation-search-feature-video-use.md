---
title: Usando a pesquisa de tradução inteligente com o AEM Assets
seo-title: Usando a pesquisa de tradução inteligente com o AEM Assets
description: A pesquisa de tradução inteligente permite a pesquisa e a descoberta de idiomas cruzados automaticamente entre AEM conteúdo, tanto Ativos quanto Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual de conteúdo.
seo-description: A pesquisa de tradução inteligente permite a pesquisa e a descoberta de idiomas cruzados automaticamente entre AEM conteúdo, tanto Ativos quanto Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual de conteúdo.
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Usando a pesquisa de tradução inteligente com o AEM Assets{#using-smart-translation-search-with-aem-assets}

A pesquisa de tradução inteligente permite a pesquisa e a descoberta de idiomas cruzados automaticamente entre AEM conteúdo, tanto Ativos quanto Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual de conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM pesquisa de tradução inteligente permite que os usuários façam buscas por conteúdo em AEM usando termos que não sejam em inglês, para que correspondam aos ativos em AEM que possuem termos equivalentes em inglês neles.

A pesquisa de tradução inteligente é um complemento perfeito para AEM tags inteligentes que são aplicadas a ativos em inglês.

Este vídeo supõe que [AEM Pesquisa de tradução inteligente](smart-translation-search-technical-video-setup.md) foi configurada.

## Como a pesquisa de tradução inteligente funciona {#how-smart-translation-search-works}

![Diagrama de Fluxo de Pesquisa de Tradução Inteligente](assets/smart-translation-search-flow.png)

1. AEM usuário realiza uma pesquisa de texto completo, fornecendo um termo de pesquisa localizado (por exemplo, o termo espanhol &quot;homem&quot;, &quot;hombre&quot;).
2. A Pesquisa de Tradução Inteligente, fornecida pelo pacote OSGi de Tradução Automática do Apache Oak, está comprometida e avalia se os termos de pesquisa fornecidos podem ser traduzidos usando os Pacotes de Idiomas registrados.
3. Todos os termos traduzidos da Etapa 2 são coletados e o query é aumentado internamente para incluí-los como termos de pesquisa. Esse conjunto aumentado de termos de pesquisa se avaliado normalmente em relação AEM índices de pesquisa que localizam correspondências relevantes.
4. Os resultados da pesquisa que correspondem ao termo original (&#39;hombre&#39;) ou ao termo traduzido (&#39;man&#39;) são coletados e retornam o usuário como os resultados da pesquisa.

## Recursos adicionais{#additional-resources}

* [Configurar a pesquisa de tradução inteligente com o AEM Assets](smart-translation-search-technical-video-setup.md)
* [Pacotes de Idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)