---
title: Usando a pesquisa de tradução inteligente com o AEM Assets
description: A Pesquisa de tradução inteligente permite a pesquisa e a descoberta entre idiomas automaticamente em todo AEM conteúdo, Ativos e Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual do conteúdo.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Usando a pesquisa de tradução inteligente com o AEM Assets{#using-smart-translation-search-with-aem-assets}

A Pesquisa de tradução inteligente permite a pesquisa e a descoberta entre idiomas automaticamente em todo AEM conteúdo, Ativos e Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual do conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Pesquisa de tradução inteligente permite que os usuários façam pesquisas por conteúdo em AEM usando termos que não sejam em inglês, para corresponder os ativos em AEM que tenham termos em inglês equivalentes neles.

A Pesquisa de tradução inteligente é um complemento perfeito para AEM tags inteligentes que são aplicadas a ativos em inglês.

Este vídeo supõe [AEM Pesquisa de tradução inteligente](smart-translation-search-technical-video-setup.md) foi configurada.

## Como a Pesquisa de tradução inteligente funciona {#how-smart-translation-search-works}

![Diagrama de Fluxo da Pesquisa de Tradução Inteligente](assets/smart-translation-search-flow.png)

1. AEM usuário realiza uma pesquisa de texto completo, fornecendo um termo de pesquisa localizado (por exemplo, o termo espanhol &quot;homem&quot;, &quot;hombre&quot;).
2. A Pesquisa de Tradução Inteligente, fornecida pelo pacote OSGi de Tradução Automática do Apache Oak, está engajada e avalia se os termos de pesquisa fornecidos podem ser traduzidos usando os Pacotes de Idiomas registrados.
3. Todos os termos traduzidos da Etapa 2 são coletados e a consulta é aumentada internamente para incluí-los como termos de pesquisa. Esse conjunto aumentado de termos de pesquisa se for avaliado normalmente em relação AEM índices de pesquisa que localizam correspondências relevantes.
4. Os resultados da pesquisa que correspondem ao termo original (&#39;hombre&#39;) ou ao termo traduzido (&#39;man&#39;) são coletados e retornam o usuário como os resultados da pesquisa.

## Recursos adicionais{#additional-resources}

* [Configurar a pesquisa de tradução inteligente com o AEM Assets](smart-translation-search-technical-video-setup.md)
* [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
