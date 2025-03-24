---
title: Guia simples de implementação de pesquisa
description: A implementação de pesquisa simples são os materiais do laboratório AEM Search Demystified de 2017. Esta página contém os materiais deste laboratório. Para um tour guiado pelo laboratório, consulte a pasta de trabalho do laboratório na seção Apresentação desta página.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 2%

---

# Guia simples de implementação de pesquisa{#simple-search-implementation-guide}

A implementação de pesquisa simples são os materiais do **Adobe Summit lab AEM Search Demystified**. Esta página contém os materiais deste laboratório. Para um tour guiado pelo laboratório, consulte a pasta de trabalho do laboratório na seção Apresentação desta página.

![Visão Geral da Arquitetura de Pesquisa](assets/l4080/simple-search-application.png)

## Materiais de apresentação {#bookmarks}

* [Pasta de trabalho do laboratório](assets/l4080/l4080-lab-workbook.pdf)
* [Apresentação](assets/l4080/l4080-presentation.pdf)

## Marcadores {#bookmarks-1}

### Ferramentas {#tools}

* [Gerenciador de Índice](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Explicar consulta](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [Gerenciador de pacotes do CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Depurador do QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Gerador de Definição de Índice Oak](https://oakutils.appspot.com/generate/index)

### Capítulos {#chapters}

*Os links de Capítulo abaixo pressupõem que os [Pacotes Iniciais](#initialpackages) estejam instalados no AEM Author em`http://localhost:4502`*

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

### Pacotes iniciais {#initial-packages}

* [Tags](assets/l4080/summit-tags.zip)
* [Pacote de aplicativo de pesquisa simples](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Pacotes de capítulo {#chapter-packages}

* [Solução do capítulo 1](assets/l4080/l4080-chapter1.zip)
* [Solução do capítulo 2](assets/l4080/l4080-chapter2.zip)
* [Solução do capítulo 3](assets/l4080/l4080-chapter3.zip)
* [Solução do capítulo 4](assets/l4080/l4080-chapter4.zip)
* [Configuração do capítulo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Capítulo 5 Solução](assets/l4080/l4080-chapter5-solution.zip)
* [Capítulo 6 Solução](assets/l4080/l4080-chapter6.zip)
* [Solução do capítulo 9](assets/l4080/l4080-chapter9.zip)

## Materiais referenciados {#reference-materials}

* [Repositório Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelos sling](https://sling.apache.org/documentation/bundles/models.html)
* [Exportador de Modelo do Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API do QueryBuilder](https://experienceleague.adobe.com/pt-br/docs)
* [Plug-in do AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Página de documentação](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correções e acompanhamento {#corrections-and-follow-up}

Correções e esclarecimentos das discussões no laboratório e respostas das perguntas de acompanhamento dos participantes.

1. **Como interromper a reindexação?**

   A reindexação pode ser interrompida por meio do MBean IndexStats disponível através do [AEM Web Console > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Execute `abortAndPause()` para anular a reindexação. Isso bloqueará o índice para reindexação até que `resume()` seja chamado.
      * Executar `resume()` reiniciará o processo de indexação.
   * Documentação: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Como os índices do oak podem suportar vários locatários?**

   O Oak permite a inserção de índices por meio da árvore de conteúdo, e esses índices serão indexados somente nessa subárvore. Por exemplo, **`/content/site-a/oak:index/cqPageLucene`** pode ser criado para indexar conteúdo somente em **`/content/site-a`.**

   Uma abordagem equivalente é usar as propriedades **`includePaths`** e **`queryPaths`** em um índice em **`/oak:index`**. Por exemplo:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   As considerações com essa abordagem são:

   * As consultas DEVEM especificar uma restrição de caminho que seja igual ao escopo do caminho de consulta do índice, ou ser um descendente dele.
   * Índices com escopo mais amplo (por exemplo, `/oak:index/cqPageLucene`) TAMBÉM indexarão os dados, resultando em assimilação duplicada e custo de uso do disco.
   * Pode exigir gerenciamento de configuração duplicado (por exemplo, adicionar as mesmas indexRules em vários índices de locatário se eles precisarem atender aos mesmos conjuntos de consultas)
   * Essa abordagem é mais adequada no nível de publicação do AEM para pesquisa de site personalizada, já que, no AEM Author, é comum que as consultas sejam executadas no alto da árvore de conteúdo para diferentes locatários (por exemplo, por meio do OmniSearch). Definições de índice diferentes podem resultar em um comportamento diferente, com base apenas na restrição de caminho.

3. **Onde está uma lista de todos os Analisadores disponíveis?**

   O Oak expõe um conjunto de elementos de configuração do analisador do lucene para uso no AEM.

   * [Documentação dos analisadores do Apache Oak](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtros](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [FiltrosCaracteres](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Como pesquisar por Páginas e Assets na mesma consulta?**

   Uma novidade no AEM 6.3 é a capacidade de consultar vários tipos de nó na mesma consulta fornecida. A seguinte consulta do QueryBuilder. Observe que cada &quot;subconsulta&quot; pode resolver para seu próprio índice. Portanto, neste exemplo, a subconsulta `cq:Page` é resolvida como `/oak:index/cqPageLucene` e a subconsulta `dam:Asset` é resolvida como `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   resultados na consulta e no plano de consulta a seguir:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Explore a consulta e os resultados por meio do [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) e do [Plug-in AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Como pesquisar vários caminhos na mesma consulta?**

   Uma novidade no AEM 6.3 é a capacidade de consultar em vários caminhos no mesmo query fornecido. A seguinte consulta do QueryBuilder. Observe que cada &quot;subconsulta&quot; pode resolver para seu próprio índice.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   resultados na seguinte consulta e plano de consulta

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Explore a consulta e os resultados por meio do [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) e do [Plug-in AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
