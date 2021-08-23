---
title: Guia de implementação de pesquisa simples
description: A implementação Simple search são os materiais do laboratório 2017 Summit AEM Search Demystified. Esta página contém os materiais deste laboratório. Para obter um tour guiado do laboratório, visualize a pasta de trabalho do Lab na seção Apresentação desta página.
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
feature: 'Pesquisar  '
topic: Desenvolvimento
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 2%

---


# Guia de implementação de pesquisa simples{#simple-search-implementation-guide}

A Implementação de pesquisa Simples são os materiais do **Adobe Summit lab AEM Search Demystified**. Esta página contém os materiais deste laboratório. Para obter um tour guiado do laboratório, visualize a pasta de trabalho do Lab na seção Apresentação desta página.

![Visão geral da arquitetura de pesquisa](assets/l4080/simple-search-application.png)

## Materiais de apresentação {#bookmarks}

* [Pasta de trabalho do laboratório](assets/l4080/l4080-lab-workbook.pdf)
* [Apresentação](assets/l4080/l4080-presentation.pdf)

## Marcadores {#bookmarks-1}

### Ferramentas {#tools}

* [Gerenciador de índice](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Explicar consulta](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene)  > /oak:index/cqPageLucene
* [Gerenciador de pacotes CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Gerador de definição de índice Oak](https://oakutils.appspot.com/generate/index)

### Capítulos {#chapters}

*Os links do Capítulo abaixo pressupõem que os  [pacotes ](#initialpackages) iniciais estejam instalados no AEM Author em`http://localhost:4502`*

* [Capítulo 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Capítulo 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Capítulo 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Capítulo 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Capítulo 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Capítulo 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Capítulo 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Capítulo 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Capítulo 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Pacotes {#packages}

### Embalagens iniciais {#initial-packages}

* [Tags](assets/l4080/summit-tags.zip)
* [Pacote do aplicativo de pesquisa simples](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Pacotes de capítulo {#chapter-packages}

* [Solução do capítulo 1](assets/l4080/l4080-chapter1.zip)
* [Solução do capítulo 2](assets/l4080/l4080-chapter2.zip)
* [Solução do capítulo 3](assets/l4080/l4080-chapter3.zip)
* [Solução do capítulo 4](assets/l4080/l4080-chapter4.zip)
* [Configuração do capítulo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Solução do capítulo 5](assets/l4080/l4080-chapter5-solution.zip)
* [Solução do capítulo 6](assets/l4080/l4080-chapter6.zip)
* [Solução do capítulo 9](assets/l4080/l4080-chapter9.zip)

## Materiais referenciados {#reference-materials}

* [Repositório Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelos sling](https://sling.apache.org/documentation/bundles/models.html)
* [Exportador de Modelo Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API do QueryBuilder](https://experienceleague.adobe.com/docs/)
* [AEM Plug-in do Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode)  ([página de documentação](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correções e acompanhamento {#corrections-and-follow-up}

Correções e esclarecimentos das discussões do laboratório e respostas para perguntas complementares dos participantes.

1. **Como parar a reindexação?**

   A reindexação pode ser interrompida por meio do MBean IndexStats disponível por meio de [AEM Console da Web > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Execute `abortAndPause()` para suspender a reindexação. Isso bloqueará o índice para reindexação adicional até que `resume()` seja chamado.
      * A execução de `resume()` reiniciará o processo de indexação.
   * Documentação: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Como os índices do oak podem suportar vários locatários?**

   O Oak suporta a inserção de índices através da árvore de conteúdo, e esses índices serão indexados somente nessa subárvore. Por exemplo, **`/content/site-a/oak:index/cqPageLucene`** pode ser criado para indexar conteúdo somente em **`/content/site-a`.**

   Uma abordagem equivalente é usar as propriedades **`includePaths`** e **`queryPaths`** em um índice em **`/oak:index`**. Por exemplo:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   As considerações com essa abordagem são:

   * As consultas DEVEM especificar uma restrição de caminho que seja igual ao escopo do caminho de consulta do índice, ou ser um descendente lá de.
   * Índices de escopo mais amplos (por exemplo `/oak:index/cqPageLucene`) TAMBÉM indexarão os dados, resultando em assimilação duplicada e custo de uso do disco.
   * Pode exigir gerenciamento de configuração duplicada (por exemplo, adicionar o mesmo indexRules em vários índices de locatários, se eles tiverem que atender aos mesmos conjuntos de query)
   * Essa abordagem é mais bem servida no nível de Publicação do AEM para pesquisa de site personalizada, como no Autor do AEM, é comum que consultas sejam executadas no alto da árvore de conteúdo para diferentes locatários (por exemplo, via OmniSearch) - definições de índice diferentes podem resultar em comportamento diferente com base apenas na restrição de caminho.


3. **Onde está uma lista de todos os Analisadores disponíveis?**

   O Oak expõe um conjunto de elementos de configuração do lucene-fornece o analisador para uso no AEM.

   * [Documentação dos Analisadores do Apache Oak](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtros](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Como pesquisar por Páginas e ativos na mesma consulta?**

   Uma novidade no AEM 6.3 é a capacidade de consultar vários tipos de nó na mesma consulta fornecida. A consulta do QueryBuilder a seguir. Observe que cada &quot;subconsulta&quot; pode resolver para seu próprio índice, portanto, neste exemplo, a subconsulta `cq:Page` resolve para `/oak:index/cqPageLucene` e a subconsulta `dam:Asset` resolve para `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   resulta no seguinte query e plano de query:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Explore a consulta e os resultados via [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Plug-in do Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Como pesquisar por vários caminhos na mesma query?**

   Uma novidade no AEM 6.3 é a capacidade de consultar vários caminhos na mesma consulta fornecida. A consulta do QueryBuilder a seguir. Observe que cada &quot;subconsulta&quot; pode resolver para seu próprio índice.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   resulta no seguinte query e plano de query

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Explore a consulta e os resultados via [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) e [AEM Plug-in do Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
