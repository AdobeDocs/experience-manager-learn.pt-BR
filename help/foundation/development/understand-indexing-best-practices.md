---
title: Práticas recomendadas de indexação no AEM
description: Saiba mais sobre as práticas recomendadas de indexação no AEM.
version: 6.4, 6.5, Cloud Service
sub-product: Experience Manager, Experience Manager Sites
feature: Search
doc-type: Article
topic: Development
role: Developer, Architect
level: Beginner
duration: 0
last-substantial-update: 2024-01-04T00:00:00Z
jira: KT-14745
thumbnail: KT-14745.jpeg
source-git-commit: 7f69fc888a7b603ffefc70d89ea470146971067e
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 0%

---


# Práticas recomendadas de indexação no AEM

Saiba mais sobre as práticas recomendadas de indexação no Adobe Experience Manager (AEM). Apache [Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/query.html) habilita a pesquisa de conteúdo no AEM e os seguintes são pontos-chave:

- Imediatamente, o AEM fornece vários índices para oferecer suporte à funcionalidade de pesquisa e consulta, por exemplo `damAssetLucene`, `cqPageLucene` e muito mais.
- Todas as definições de índice são armazenadas no repositório em `/oak:index` nó.
- O AEM as a Cloud Service só oferece suporte a índices Oak Lucene.
- A configuração do índice deve ser gerenciada na base de código do projeto AEM e implantada usando os pipelines CI/CD do Cloud Manager.
- Se vários índices estiverem disponíveis para uma determinada consulta, a variável **índice com o menor custo estimado é usado**.
- Se nenhum índice estiver disponível para uma determinada consulta, a árvore de conteúdo será percorrida para encontrar o conteúdo correspondente. No entanto, o limite padrão via `org.apache.jackrabbit.oak.query.QueryEngineSettingsService` é atravessar apenas 10.000 nós.
- Os resultados de um query são **filtrado por fim** para garantir que o usuário atual tenha acesso de leitura. Isso significa que os resultados da consulta podem ser menores que o número de nós indexados.
- A reindexação do repositório após as alterações de definição de índice requer tempo e depende do tamanho do repositório.

Para ter uma funcionalidade de pesquisa eficiente e correta que não afete o desempenho da instância do AEM, é importante entender as práticas recomendadas de indexação.

## Índice personalizado vs. OOTB

Às vezes, você deve criar índices personalizados para dar suporte aos requisitos de pesquisa. No entanto, siga as diretrizes abaixo antes de criar índices personalizados:

- Entenda os requisitos de pesquisa e verifique se os índices OOTB podem dar suporte aos requisitos de pesquisa. Uso **Ferramenta de desempenho da consulta**, disponível em [SDK local](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) e AEMCS por meio do Console do desenvolvedor ou `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`.

- Defina uma consulta ideal, use o [otimização de consultas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html?#optimizing-queries) fluxograma e [Folha de características de consulta JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) para referência.

- Se os índices OOTB não forem compatíveis com os requisitos de pesquisa, você terá duas opções. No entanto, reveja a [Dicas para Criar Índices Eficientes](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#should-i-create-an-index)
   - Personalizar o índice OOTB: opção preferencial, pois é fácil de manter e atualizar.
   - Índice totalmente personalizado: somente se a opção acima não funcionar.

### Personalizar o índice OOTB

- Entrada **AEMCS**, ao personalizar o uso do índice OOTB **\&lt;ootbindexname>-\&lt;productversion>-custom-\&lt;customversion>** convenção de nomenclatura. Por exemplo, `cqPageLucene-custom-1` ou `damAssetLucene-8-custom-1`. Isso ajuda a mesclar a definição de índice personalizado sempre que o índice OOTB é atualizado. Consulte [Alterações nos índices prontos para uso](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#changes-to-out-of-the-box-indexes) para obter mais detalhes.

- Entrada **AEM 6.X**, o nome acima _não funciona_ No entanto, basta atualizar o índice OOTB com propriedades adicionais no `indexRules` nó.

- Sempre copie a definição de índice OOTB mais recente da instância AEM usando o Gerenciador de pacotes CRX DE (/crx/packmgr/), renomeie-a e adicione personalizações dentro do arquivo XML.

- Armazenar definição de índice no projeto AEM em `ui.apps/src/main/content/jcr_root/_oak_index` e implantá-lo usando os pipelines de CI/CD do Cloud Manager. Consulte [Implantação de definições de índice personalizadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) para obter mais detalhes.

### Índice totalmente personalizado

A criação de um índice totalmente personalizado deve ser a última opção e somente se a opção acima não funcionar.

- Ao criar um índice totalmente personalizado, use **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** convenção de nomenclatura. Por exemplo, `wknd.adventures-1-custom-1`. Isso ajuda a evitar conflitos de nomenclatura. Aqui, `wknd` é o prefixo e `adventures` é o nome do índice personalizado. Essa convenção é aplicável ao AEM 6.X e ao AEMCS e ajuda a preparar a migração futura para o AEMCS.

- O AEM CS é compatível apenas com os índices Lucene, portanto, para se preparar para a migração futura para o AEM, sempre use os índices Lucene. Consulte [Índices Lucene versus Índices de propriedades](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#lucene-or-property-indexes) para obter mais detalhes.

- Evite criar um índice personalizado no mesmo tipo de nó do índice OOTB. Em vez disso, personalize o índice OOTB com propriedades adicionais no `indexRules` nó. Por exemplo, não crie um índice personalizado no `dam:Asset` tipo de nó, mas personalizar o OOTB `damAssetLucene` índice. _Essa tem sido uma causa básica comum de problemas funcionais e de desempenho_.

- Além disso, evite adicionar vários tipos de nó, por exemplo `cq:Page` e `cq:Tag` nas regras de indexação (`indexRules`). Em vez disso, crie índices separados para cada tipo de nó.

- Como mencionado na seção acima, armazene a definição do índice no projeto AEM em `ui.apps/src/main/content/jcr_root/_oak_index` e implantá-lo usando os pipelines de CI/CD do Cloud Manager. Consulte [Implantação de definições de índice personalizadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) para obter mais detalhes.

- As diretrizes de definição do índice são:
   - O tipo de nó (`jcr:primaryType`) deve ser `oak:QueryIndexDefinition`
   - O tipo de índice (`type`) deve ser `lucene`
   - A propriedade assíncrona (`async`) deve ser `async,nrt`
   - Uso `includedPaths` e evitar `excludedPaths` propriedade. Sempre definir `queryPaths` para o mesmo valor que `includedPaths` valor.
   - Para aplicar a restrição de caminho, use `evaluatePathRestrictions` propriedade e defina-a como `true`.
   - Uso `tags` propriedade para marcar o índice e, durante a consulta, especificar esse valor de tags para usar o índice. A sintaxe de consulta geral é `<query> option(index tag <tagName>)`.

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

A imagem abaixo mostra a definição de índice OOTB e personalizados, destacando o `tags` propriedade, ambos os índices usam o mesmo `visualSimilaritySearch` valor.

![Uso indevido da propriedade de tags](./assets/understand-indexing-best-practices/incorrect-tags-property.png)

##### Análise

Este é um uso inadequado do `tags` no índice personalizado. O mecanismo de consulta Oak escolhe o índice personalizado sobre a causa do índice OOTB do custo estimado mais baixo.

A maneira correta é personalizar o índice OOTB e adicionar outras propriedades na `indexRules` nó. Consulte [Personalização do índice OOTB](#customize-the-ootb-index) para obter mais detalhes.

#### Índice no `dam:Asset` tipo de nó

A imagem abaixo mostra o índice personalizado para o `dam:Asset` tipo de nó com o `includedPaths` propriedade definida para um caminho específico.

![Índice no tipo de nó dam:Asset](./assets/understand-indexing-best-practices/index-for-damAsset-type.png)

##### Análise

Se você executar omnisearch em Ativos, ela retornará resultados incorretos porque o índice personalizado tem custo estimado mais baixo.

Não crie um índice personalizado no `dam:Asset` tipo de nó, mas personalizar o OOTB `damAssetLucene` índice com propriedades adicionais no `indexRules` nó.

#### Vários tipos de nó nas regras de indexação

A imagem abaixo mostra o índice personalizado com vários tipos de nó sob o `indexRules` nó.

![Vários tipos de nó sob as regras de indexação](./assets/understand-indexing-best-practices/multiple-nodetypes-in-index.png)

##### Análise

Não é recomendável adicionar vários tipos de nó em um único índice. No entanto, convém adicionar tipos de nó de índice no mesmo índice se os tipos de nó estiverem intimamente relacionados, por exemplo, `cq:Page` e `cq:PageContent`.

Uma solução válida é personalizar o OOTB `cqPageLucene` e `damAssetLucene` índice, adicione propriedades adicionais no existente `indexRules` nó.

#### Ausência de `queryPaths` propriedade

A imagem abaixo mostra o índice personalizado (sem seguir a convenção de nomenclatura) sem `queryPaths` propriedade.

![Ausência da propriedade queryPaths](./assets/understand-indexing-best-practices/absense-of-queryPaths-prop.png)

##### Análise

Sempre definir `queryPaths` para o mesmo valor que `includedPaths` valor. Além disso, para aplicar a restrição de caminho, defina `evaluatePathRestrictions` propriedade para `true`.

#### Consulta com tag de índice

A imagem abaixo mostra o índice personalizado com `tags` propriedade e como usá-la durante a consulta.

![Consulta com tag de índice](./assets/understand-indexing-best-practices/tags-prop-usage.png)

```
/jcr:root/content/dam//element(*,dam:Asset)[(jcr:content/@contentFragment = 'true' and jcr:contains(., '/content/sitebuilder/test/mysite/live/ja-jp/mypage'))]order by @jcr:created descending option (index tag assetPrefixNodeNameSearch)
```

##### Análise

Demonstra como definir configurações corretas e não conflitantes `tags` no índice e usá-lo durante a consulta. A sintaxe de consulta geral é `<query> option(index tag <tagName>)`. Consulte também [Tag de Índice de Opção de Consulta](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#query-option-index-tag)

#### Índice personalizado

A imagem abaixo mostra o índice personalizado com `suggestion` para obter a funcionalidade de pesquisa avançada.

![Índice personalizado](./assets/understand-indexing-best-practices/custom-index-with-suggestion-node.png)

##### Análise

É um caso de uso válido para criar um índice personalizado para o [pesquisa avançada](https://jackrabbit.apache.org/oak/docs/query/lucene.html#advanced-search-features) funcionalidade. No entanto, o nome do índice deve seguir o **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** convenção de nomenclatura.


## Ferramentas úteis

Vamos analisar algumas ferramentas que podem ajudá-lo a definir, analisar e otimizar os índices.

### Ferramenta de criação de índice

A variável [Gerador de definição de índice Oak](https://oakutils.appspot.com/generate/index) ajuda da ferramenta **para gerar a definição do índice** com base nas consultas de entrada. Criar um índice personalizado é um bom ponto de partida.

### Ferramenta Analisar índice

A variável [Analisador de definição de índice](https://oakutils.appspot.com/analyze/index) ajuda da ferramenta **para analisar a definição do índice** e fornece recomendações para melhorar a definição do índice.

### Ferramenta de desempenho de consulta

O OOTB _Ferramenta de desempenho da consulta_ disponível em [SDK local](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) e AEMCS por meio do Console do desenvolvedor ou `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell` ajuda **para analisar o desempenho da consulta** e [Folha de características de consulta JCR](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) para definir a consulta ideal.

### Dicas e ferramentas para solução de problemas

A maioria dos itens abaixo é aplicável para AEM 6.X e solução de problemas local.

- Gerenciador de índice disponível em `http://host:port/libs/granite/operations/content/diagnosistools/indexManager.html` para obter informações de índice, como tipo, última atualização, tamanho.

- Registro detalhado de pacotes Java™ relacionados à consulta e indexação do Oak, como `org.apache.jackrabbit.oak.plugins.index`, `org.apache.jackrabbit.oak.query`, e `com.day.cq.search` via `http://host:port/system/console/slinglog` para solução de problemas.

- MBean JMX de _IndexStats_ tipo disponível em `http://host:port/system/console/jmx` para obter informações de índice, como status, progresso ou estatísticas relacionadas à indexação assíncrona. Também fornece _FailedIndexStats_, se não houver resultados aqui, significa que nenhum índice está corrompido. AsyncIndexerService marca qualquer índice que não seja atualizado por 30 minutos (configurável) como corrompido e interrompe a indexação. Se uma consulta não estiver fornecendo os resultados esperados, é útil que os desenvolvedores verifiquem isso antes de prosseguir com a reindexação, pois a reindexação é computacionalmente cara e demorada.

- MBean JMX de _LuceneIndex_ tipo disponível em `http://host:port/system/console/jmx` para estatísticas do Índice Lucene, como tamanho, número de documentos por definição de índice.

- MBean JMX de _QueryStat_ tipo disponível em `http://host:port/system/console/jmx` para estatísticas de consulta do Oak, incluindo consultas lentas e populares com detalhes como consulta, tempo de execução.

## Recursos adicionais

Consulte a seguinte documentação para obter mais informações:

- [Consultas e indexação do Oak](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/deploying/queries-and-indexing.html)
- [Práticas recomendadas de consulta e indexação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html)
- [Práticas recomendadas para consultas e indexação](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html)
