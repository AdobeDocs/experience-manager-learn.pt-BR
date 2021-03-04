---
title: Usar a pesquisa de tradução inteligente com o AEM Assets
description: A Pesquisa de tradução inteligente permite a pesquisa e descoberta entre idiomas automaticamente no conteúdo do AEM, tanto Ativos quanto Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual do conteúdo.
version: 6.3, 6.4, 6.5
feature: 'Pesquisar  '
topic: Gerenciamento de conteúdo
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# Usando a pesquisa de tradução inteligente com o AEM Assets{#using-smart-translation-search-with-aem-assets}

A Pesquisa de tradução inteligente permite a pesquisa e descoberta entre idiomas automaticamente no conteúdo do AEM, tanto Ativos quanto Páginas, suportando mais de 50 idiomas e reduzindo a necessidade de tradução manual do conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

A pesquisa de tradução inteligente do AEM permite que os usuários façam pesquisas por conteúdo no AEM usando termos que não sejam em inglês, para corresponder aos ativos no AEM que têm termos em inglês equivalentes.

A Pesquisa de tradução inteligente é um complemento perfeito para as Tags inteligentes do AEM, que são aplicadas a ativos em inglês.

Este vídeo supõe que a [Pesquisa de tradução inteligente do AEM](smart-translation-search-technical-video-setup.md) foi configurada.

## Como a Pesquisa de tradução inteligente funciona {#how-smart-translation-search-works}

![Diagrama de Fluxo da Pesquisa de Tradução Inteligente](assets/smart-translation-search-flow.png)

1. O usuário do AEM realiza uma pesquisa de texto completo, fornecendo um termo de pesquisa localizado (por exemplo, o termo espanhol &quot;homem&quot;, &quot;hombre&quot;).
2. A Pesquisa de Tradução Inteligente, fornecida pelo pacote OSGi de Tradução Automática do Apache Oak, está engajada e avalia se os termos de pesquisa fornecidos podem ser traduzidos usando os Pacotes de Idiomas registrados.
3. Todos os termos traduzidos da Etapa 2 são coletados e a consulta é aumentada internamente para incluí-los como termos de pesquisa. Esse conjunto aumentado de termos de pesquisa se for avaliado normalmente em relação aos índices de pesquisa do AEM, localizando correspondências relevantes.
4. Os resultados da pesquisa que correspondem ao termo original (&#39;hombre&#39;) ou ao termo traduzido (&#39;man&#39;) são coletados e retornam o usuário como os resultados da pesquisa.

## Recursos adicionais{#additional-resources}

* [Configurar a pesquisa de tradução inteligente com o AEM Assets](smart-translation-search-technical-video-setup.md)
* [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)