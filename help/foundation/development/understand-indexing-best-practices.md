---
title: Práticas recomendadas de indexação no AEM
description: Saiba mais sobre as práticas recomendadas de indexação no AEM.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
sub-product: Experience Manager, Experience Manager Sites
feature: Search
doc-type: Article
topic: Development
role: Developer, Architect
level: Beginner
duration: 373
last-substantial-update: 2024-01-04T00:00:00Z
jira: KT-14745
thumbnail: KT-14745.jpeg
exl-id: 3fd4c404-18e9-44e5-958f-15235a3091d5
source-git-commit: 7ada3c2e7deb414b924077a5d2988db16f28712c
workflow-type: tm+mt
source-wordcount: '1693'
ht-degree: 0%

---

# Práticas recomendadas de indexação no AEM

Saiba mais sobre as práticas recomendadas de indexação no Adobe Experience Manager (AEM). O Apache [Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/query.html) habilita a pesquisa de conteúdo no AEM e os seguintes são pontos-chave:

- Pronto para uso, o AEM fornece vários índices para oferecer suporte à funcionalidade de pesquisa e consulta, por exemplo `damAssetLucene`, `cqPageLucene` e muito mais.
- Todas as definições de índice são armazenadas no repositório no nó `/oak:index`.
- O AEM as a Cloud Service só oferece suporte a índices Oak Lucene.
- A configuração do índice deve ser gerenciada na base de código do projeto do AEM e implantada usando os pipelines de CI/CD do Cloud Manager.
- Se vários índices estiverem disponíveis para uma determinada consulta, o **índice com o menor custo estimado será usado**.
- Se nenhum índice estiver disponível para uma determinada consulta, a árvore de conteúdo será percorrida para encontrar o conteúdo correspondente. No entanto, o limite padrão via `org.apache.jackrabbit.oak.query.QueryEngineSettingsService` é percorrer apenas 100.000 nós.
- Os resultados de uma consulta são **filtrados por último** para garantir que o usuário atual tenha acesso de leitura. Isso significa que os resultados da consulta podem ser menores que o número de nós indexados.
- A reindexação do repositório após as alterações de definição de índice requer tempo e depende do tamanho do repositório.

Para ter uma funcionalidade de pesquisa eficiente e correta que não afete o desempenho da instância do AEM, é importante entender as práticas recomendadas de indexação.

## Índice personalizado vs. OOTB

Às vezes, você deve criar índices personalizados para dar suporte aos requisitos de pesquisa. No entanto, siga as diretrizes abaixo antes de criar índices personalizados:

- Entenda os requisitos de pesquisa e verifique se os índices OOTB podem dar suporte aos requisitos de pesquisa. Use a **Ferramenta de Desempenho de Consulta**, disponível em [SDK local](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) e AEMCS via Developer Console ou `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`.

- Defina uma consulta ideal, use o [fluxograma de otimização de consultas](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices) e a [Folha de características de consulta JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf) como referência.

- Se os índices OOTB não forem compatíveis com os requisitos de pesquisa, você terá duas opções. No entanto, examine as [Dicas para Criar Índices Eficientes](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing)
   - Personalizar o índice OOTB: opção preferencial, pois é fácil de manter e atualizar.
   - Índice totalmente personalizado: somente se a opção acima não funcionar.

### Personalizar o índice OOTB

- No **AEMCS**, ao personalizar o índice OOTB, use a convenção de nomenclatura **\&lt;OOTBIndexName>-\&lt;productVersion>-custom-\&lt;customVersion>**. Por exemplo, `cqPageLucene-custom-1` ou `damAssetLucene-8-custom-1`. Isso ajuda a mesclar a definição de índice personalizado sempre que o índice OOTB é atualizado. Consulte [Alterações em índices prontos para uso](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/operations/indexing) para obter mais detalhes.

- No **AEM 6.X**, a nomenclatura _acima não funciona_. No entanto, basta atualizar o índice OOTB com as propriedades necessárias no nó `indexRules`.

- Sempre copie a definição de índice OOTB mais recente da instância do AEM usando o Gerenciador de pacotes do CRX DE (/crx/packmgr/), renomeie-a e adicione personalizações dentro do arquivo XML.

- Armazene a definição de índice no projeto do AEM em `ui.apps/src/main/content/jcr_root/_oak_index` e implante-a usando os pipelines de CI/CD do Cloud Manager. Consulte [Implantando Definições de Índice Personalizadas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/operations/indexing) para obter mais detalhes.

### Índice totalmente personalizado

A criação de um índice totalmente personalizado deve ser a última opção e somente se a opção acima não funcionar.

- Ao criar um índice totalmente personalizado, use **\&lt;prefix>.\&lt;customIndexName>-\&lt;version>-custom-\&lt;customVersion>** convenção de nomenclatura. Por exemplo, `wknd.adventures-1-custom-1`. Isso ajuda a evitar conflitos de nomenclatura. Aqui, `wknd` é o prefixo e `adventures` é o nome de índice personalizado. Essa convenção é aplicável ao AEM 6.X e AEMCS e ajuda a preparar a migração futura para o AEMCS.

- O AEM CS é compatível apenas com os índices Lucene, portanto, para se preparar para a migração futura para o AEM, sempre use os índices Lucene. Consulte [Índices Lucene versus Índices de propriedades](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing) para obter mais detalhes.

- Evite criar um índice personalizado no mesmo tipo de nó do índice OOTB. Em vez disso, personalize o índice OOTB com as propriedades necessárias no nó `indexRules`. Por exemplo, não crie um índice personalizado no tipo de nó `dam:Asset`, mas personalize o índice `damAssetLucene` OOTB. _Foi uma causa raiz comum de problemas de desempenho e funcionais_.

- Além disso, evite adicionar vários tipos de nó, por exemplo `cq:Page` e `cq:Tag`, sob o nó de regras de indexação (`indexRules`). Em vez disso, crie índices separados para cada tipo de nó.

- Como mencionado na seção acima, armazene a definição do índice no projeto do AEM em `ui.apps/src/main/content/jcr_root/_oak_index` e implante-a usando os pipelines de CI/CD do Cloud Manager. Consulte [Implantando Definições de Índice Personalizadas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/operations/indexing) para obter mais detalhes.

- As diretrizes de definição do índice são:
   - O tipo de nó (`jcr:primaryType`) deve ser `oak:QueryIndexDefinition`
   - O tipo de índice (`type`) deve ser `lucene`
   - A propriedade assíncrona (`async`) deve ser `async,nrt`
   - Use `includedPaths` e evite a propriedade `excludedPaths`. Sempre defina o valor `queryPaths` com o mesmo valor que o valor `includedPaths`.
   - Para impor a restrição de caminho, use a propriedade `evaluatePathRestrictions` e defina-a como `true`.
   - Use a propriedade `tags` para marcar o índice e, durante a consulta, especifique esse valor de marcas para usar o índice. A sintaxe de consulta geral é `<query> option(index tag <tagName>)`.

  ```xml
  /oak:index/wknd.adventures-1-custom-1
      - jcr:primaryType = "oak:QueryIndexDefinition"
      - type = "lucene"
      - compatVersion = 2
      - async = ["async", "nrt"]
      - includedPaths = ["/content/wknd"]
      - queryPaths = ["/content/wknd"]
      - evaluatePathRestrictions = true
      - tags = ["customAdvSearch"]
  ...
  ```

### Exemplos

Para entender as práticas recomendadas, vamos analisar alguns exemplos.

#### Uso indevido da propriedade de tags

A imagem abaixo mostra a definição de índice OOTB e personalizado, destacando a propriedade `tags`. Ambos os índices usam o mesmo valor `visualSimilaritySearch`.

![Uso inadequado da propriedade de marcas](./assets/understand-indexing-best-practices/incorrect-tags-property.png)

##### Análise

Este é um uso inadequado da propriedade `tags` no índice personalizado. O mecanismo de consulta do Oak escolhe o índice personalizado sobre a causa do índice OOTB do custo estimado mais baixo.

A maneira correta é personalizar o índice OOTB e adicionar as propriedades necessárias no nó `indexRules`. Consulte [Personalizando o índice OOTB](#customize-the-ootb-index) para obter mais detalhes.

#### Índice no tipo de nó `dam:Asset`

A imagem abaixo mostra o índice personalizado para o tipo de nó `dam:Asset` com a propriedade `includedPaths` definida como um caminho específico.

![Índice no nodetype dam:Asset](./assets/understand-indexing-best-practices/index-for-damAsset-type.png)

##### Análise

Se você executar o omnisearch no Assets, ele retornará resultados incorretos porque o índice personalizado tem um custo estimado mais baixo.

Não crie um índice personalizado no tipo de nó `dam:Asset`, mas personalize o índice `damAssetLucene` OOTB com as propriedades necessárias no nó `indexRules`.

#### Vários tipos de nó nas regras de indexação

A imagem abaixo mostra o índice personalizado com vários tipos de nó sob o nó `indexRules`.

![Vários tipos de nó sob as regras de indexação](./assets/understand-indexing-best-practices/multiple-nodetypes-in-index.png)

##### Análise

Não é recomendável adicionar vários tipos de nó em um único índice. No entanto, convém adicionar tipos de nó de índice no mesmo índice se os tipos de nó estiverem intimamente relacionados, por exemplo, `cq:Page` e `cq:PageContent`.

Uma solução válida é personalizar o índice OOTB `cqPageLucene` e `damAssetLucene`, adicionar as propriedades necessárias sob o nó `indexRules` existente.

#### Ausência da propriedade `queryPaths`

A imagem abaixo mostra o índice personalizado (não seguindo também a convenção de nomenclatura) sem a propriedade `queryPaths`.

![Ausência da propriedade queryPaths](./assets/understand-indexing-best-practices/absense-of-queryPaths-prop.png)

##### Análise

Sempre defina o valor `queryPaths` com o mesmo valor que o valor `includedPaths`. Além disso, para impor a restrição de caminho, defina a propriedade `evaluatePathRestrictions` como `true`.

#### Consulta com tag de índice

A imagem abaixo mostra o índice personalizado com a propriedade `tags` e como usá-lo durante a consulta.

![Consultando com marca de índice](./assets/understand-indexing-best-practices/tags-prop-usage.png)

```
/jcr:root/content/dam//element(*,dam:Asset)[(jcr:content/@contentFragment = 'true' and jcr:contains(., '/content/sitebuilder/test/mysite/live/ja-jp/mypage'))]order by @jcr:created descending option (index tag assetPrefixNodeNameSearch)
```

##### Análise

Demonstra como definir um valor de propriedade `tags` não conflitante e correto no índice e usá-lo durante a consulta. A sintaxe de consulta geral é `<query> option(index tag <tagName>)`. Consulte também [Marca de Índice de Opção de Consulta](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#query-option-index-tag)

#### Índice personalizado

A imagem abaixo mostra o índice personalizado com o nó `suggestion` para obter a funcionalidade de pesquisa avançada.

![Índice personalizado](./assets/understand-indexing-best-practices/custom-index-with-suggestion-node.png)

##### Análise

É um caso de uso válido para criar um índice personalizado para a funcionalidade de [pesquisa avançada](https://jackrabbit.apache.org/oak/docs/query/lucene.html#advanced-search-features). No entanto, o nome do índice deve seguir o **\&lt;prefix>.\&lt;customIndexName>-\&lt;version>-custom-\&lt;customVersion>** convenção de nomenclatura.

## Otimização de índice ao desabilitar o Apache Tika

O AEM usa o [Apache Tika](https://tika.apache.org/) para _extrair conteúdo de metadados e texto de arquivos_ de tipos como PDF, Word, Excel e muito mais. O conteúdo extraído é armazenado no repositório e indexado pelo índice Oak Lucene.

Às vezes, os usuários não exigem a capacidade de pesquisar no conteúdo de um arquivo/ativo, nesses casos, você pode melhorar o desempenho da indexação desativando o Apache Tika. As vantagens são:

- Indexação mais rápida
- Redução de tamanho do índice
- Menos uso de hardware

>[!CAUTION]
>
>Antes de desabilitar o Apache Tika, verifique se os requisitos de pesquisa não exigem a capacidade de pesquisar no conteúdo de um ativo.


### Desativar por tipo MIME

Para desativar o Apache Tika por tipo MIME, use as seguintes etapas:

- Adicione o nó `tika` do tipo `nt:unstructured` sob a definição de índice OOBT ou personalizado. No exemplo a seguir, o tipo mime do PDF está desabilitado para o índice `damAssetLucene` OOTB.

```xml
/oak:index/damAssetLucene
    - jcr:primaryType = "oak:QueryIndexDefinition"
    - type = "lucene"
    ...
    <tika jcr:primaryType="nt:unstructured">
        <config.xml/>
    </tika>
```

- Adicione o `config.xml` com os seguintes detalhes no nó `tika`.

```xml
<properties>
  <parsers>
    <parser class="org.apache.tika.parser.EmptyParser">
      <mime>application/pdf</mime>
      <!-- Add more mime types to disable -->
  </parsers>
</properties>
```

- Para atualizar o índice armazenado, defina a propriedade `refresh` como `true` no nó de definição de índice. Consulte [Propriedades de Definição de Índice](https://jackrabbit.apache.org/oak/docs/query/lucene.html#index-definition:~:text=Defaults%20to%2010000-,refresh,-Optional%20boolean%20property) para obter mais detalhes.

A imagem a seguir mostra o índice `damAssetLucene` OOTB com o nó `tika` e o arquivo `config.xml` que desabilita o PDF e outros tipos MIME.

![Índice damAssetLucene de OOTB com nó tika](./assets/understand-indexing-best-practices/ootb-index-with-tika-node.png)

### Desativar completamente

Para desativar completamente o Apache Tika, siga as etapas abaixo:

- Adicione a propriedade `includePropertyTypes` em `/oak:index/<INDEX-NAME>/indexRules/<NODE-TYPE>` e defina o valor como `String`. Por exemplo, na imagem abaixo, a propriedade `includePropertyTypes` é adicionada ao tipo de nó `dam:Asset` do índice OOBT `damAssetLucene`.

![Propriedade IncludePropertyTypes](./assets/understand-indexing-best-practices/includePropertyTypes-prop.png)

- Adicione `data` com as propriedades abaixo do nó `properties`, verifique se é o primeiro nó acima da definição de propriedade. Por exemplo, veja a imagem abaixo:

```xml
/oak:index/<INDEX-NAME>/indexRules/<NODE-TYPE>/properties/data
    - jcr:primaryType = "nt:unstructured"
    - type = "String"
    - name = "jcr:data"
    - nodeScopeIndex = false
    - propertyIndex = false
    - analyze = false
```

![Propriedade de dados](./assets/understand-indexing-best-practices/data-prop.png)

- Reindexe a definição de índice atualizada definindo a propriedade `reindex` como `true` no nó de definição de índice.

## Ferramentas úteis

Vamos analisar algumas ferramentas que podem ajudá-lo a definir, analisar e otimizar os índices.

### Ferramenta de criação de índice

A ferramenta [Gerador de Definição de Índice Oak](https://oakutils.appspot.com/generate/index) ajuda **a gerar a definição de índice** com base nas consultas de entrada. Criar um índice personalizado é um bom ponto de partida.

### Ferramenta Analisar índice

A ferramenta [Analisador de Definição de Índice](https://oakutils.appspot.com/analyze/index) ajuda o **a analisar a definição do índice** e fornece recomendações para melhorar a definição do índice.

### Ferramenta de desempenho de consulta

A _Ferramenta de Desempenho de Consulta_ do OOTB, disponível em [SDK local](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) e AEMCS por meio da Developer Console ou do `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`, ajuda a **analisar o desempenho da consulta** e a [Folha de características de consulta JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) para definir a consulta ideal.

### Dicas e ferramentas para solução de problemas

A maioria dos itens abaixo é aplicável ao AEM 6.X e para fins de solução de problemas local.

- Gerenciador de Índice disponível em `http://host:port/libs/granite/operations/content/diagnosistools/indexManager.html` para obter informações de índice, como tipo, última atualização, tamanho.

- Registro detalhado de pacotes Java™ relacionados à consulta e indexação do Oak, como `org.apache.jackrabbit.oak.plugins.index`, `org.apache.jackrabbit.oak.query` e `com.day.cq.search` via `http://host:port/system/console/slinglog`, para solução de problemas.

- MBean JMX do tipo _IndexStats_ disponível em `http://host:port/system/console/jmx` para obter informações de índice, como status, progresso ou estatísticas relacionadas à indexação assíncrona. Também fornece _FailingIndexStats_, se não houver resultados aqui, significa que nenhum índice está corrompido. AsyncIndexerService marca qualquer índice que não seja atualizado por 30 minutos (configurável) como corrompido e interrompe a indexação. Se uma consulta não estiver fornecendo os resultados esperados, é útil que os desenvolvedores verifiquem isso antes de prosseguir com a reindexação, pois a reindexação é computacionalmente cara e demorada.

- MBean JMX do tipo _LuceneIndex_ disponível em `http://host:port/system/console/jmx` para estatísticas do Índice Lucene, como tamanho, número de documentos por definição de índice.

- MBean JMX do tipo _QueryStat_ disponível em `http://host:port/system/console/jmx` para Estatísticas de consulta do Oak, incluindo consultas lentas e populares com detalhes como consulta, tempo de execução.

## Recursos adicionais

Consulte a seguinte documentação para obter mais informações:

- [Consultas e indexação do Oak](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/deploying/deploying/queries-and-indexing)
- [Práticas recomendadas de consulta e indexação](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices)
- [Práticas recomendadas para consultas e indexação](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing)

