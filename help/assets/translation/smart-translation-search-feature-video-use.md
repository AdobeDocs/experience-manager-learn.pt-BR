---
title: Uso da pesquisa de tradução inteligente com o AEM Assets
description: A Pesquisa inteligente de tradução permite a pesquisa e descoberta entre idiomas automaticamente em conteúdo AEM, em ativos e páginas, oferecendo suporte a mais de 50 idiomas e reduzindo a necessidade de tradução manual de conteúdo.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 211
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Uso da pesquisa de tradução inteligente com o AEM Assets{#using-smart-translation-search-with-aem-assets}

A Pesquisa inteligente de tradução permite a pesquisa e descoberta entre idiomas automaticamente em conteúdo AEM, em ativos e páginas, oferecendo suporte a mais de 50 idiomas e reduzindo a necessidade de tradução manual de conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

A pesquisa de tradução inteligente do AEM permite que os usuários realizem pesquisas de conteúdo no AEM usando termos que não sejam em inglês, para corresponder aos ativos no AEM que têm termos em inglês equivalentes.

A pesquisa de tradução inteligente é um complemento perfeito das Tags inteligentes de AEM, que são aplicadas aos ativos em inglês.

Este vídeo presume [Pesquisa de tradução inteligente do AEM](smart-translation-search-technical-video-setup.md) foi configurado.

## Como a pesquisa inteligente de tradução funciona {#how-smart-translation-search-works}

![Diagrama de fluxo de pesquisa de tradução inteligente](assets/smart-translation-search-flow.png)

1. O usuário do AEM faz uma pesquisa de texto completo, fornecendo um termo de pesquisa localizado (por exemplo, o termo espanhol para &quot;man&quot;, &quot;hombre&quot;).
2. A Pesquisa de tradução inteligente, fornecida pelo pacote OSGi de tradução automática do Apache Oak, está envolvida e avalia se os termos de pesquisa fornecidos podem ser traduzidos usando os Pacotes de idioma registrados.
3. Todos os termos traduzidos da Etapa #2 são coletados e a consulta é aumentada internamente para incluí-los como termos de pesquisa. Esse conjunto aumentado de termos de pesquisa se avaliado normalmente em relação aos índices de pesquisa AEM que localizam correspondências relevantes.
4. Os resultados da pesquisa que correspondem ao termo original (&quot;hombre&quot;) ou ao termo traduzido (&quot;man&quot;) são coletados e retornados ao usuário como os resultados da pesquisa.

## Recursos adicionais{#additional-resources}

* [Configurar a pesquisa de tradução inteligente com o AEM Assets](smart-translation-search-technical-video-setup.md)
* [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
