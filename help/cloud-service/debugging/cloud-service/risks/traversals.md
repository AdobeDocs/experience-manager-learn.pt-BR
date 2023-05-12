---
title: Avisos de passagem em AEM as a Cloud Service
description: Saiba como mitigar avisos de travessia AEM as a Cloud Service.
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
source-git-commit: a439c72a7b080633d3777eefad3b47f01c92b970
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 10%

---

# Avisos de travessia

>[!TIP]
>Marque esta página como favorito para referência futura.

_O que são avisos transversais?_

Os avisos de travessia são __aemerror__ instruções de log indicando que consultas com baixo desempenho estão sendo executadas no serviço de publicação do AEM. Os avisos transversais normalmente manifestam-se em AEM de duas formas:

1. __Consultas lentas__ que não usam índices, resultando em tempos de resposta lentos.
1. __Falhas nas consultas__, que lança uma `RuntimeNodeTraversalException`, resultando em uma experiência quebrada.

Permitir que avisos de travessia fiquem desmarcados retarda o desempenho AEM e pode resultar em experiências quebradas para seus usuários.

## Como resolver avisos transversais

A mitigação de avisos de travessia pode ser abordada usando três etapas simples: analisar, ajustar e verificar. Esperar várias iterações de ajuste e verificação antes de identificar os ajustes ideais.

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="Analisar" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="Analisar">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Analise o problema</p>
               <p class="is-size-6">Identifique e entenda o que as consultas estão atravessando.</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Analisar</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="Ajustar " tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="Ajustar ">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Ajustar o código ou a configuração</p>
               <p class="is-size-6">Atualize as consultas e os índices para evitar os traversários da consulta.</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ajustar</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="Verificar" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="Verificar">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Verificar se os ajustes funcionaram</p>                       
               <p class="is-size-6">Verifique se as alterações em queries e índices removem os traversais.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verificar</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. Analisar{#analyze}

Primeiro, identifique quais serviços do AEM Publish estão exibindo avisos transversais. Para fazer isso, no Cloud Manager, [baixar serviços de publicação&quot; `aemerror` logs](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target="_blank"} de todos os ambientes (desenvolvimento, estágio e produção) do passado __três dias__.

![Baixar AEM registros as a Cloud Service](./assets/traversals/download-logs.jpg)

Abra os arquivos de log e procure a classe Java™ `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. O log contendo avisos de travessia contém uma série de instruções semelhantes a:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

Dependendo do contexto da execução do query, as declarações de log podem conter informações úteis sobre o originador do query:

+ URL da solicitação HTTP associada à execução da consulta

   + Exemplo: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Sintaxe de consulta Oak

   + Exemplo: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ Consulta XPath

   + Exemplo: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Código que executa a consulta

   + Exemplo:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Falhas nas consultas__ são seguidas por um `RuntimeNodeTraversalException` instrução, semelhante a:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. Ajustar{#adjust}

Depois que as consultas ofensivas e seu código de chamada forem descobertos, os ajustes devem ser feitos. Dois tipos de ajustes podem ser feitos para atenuar avisos de travessia:

### Ajustar o query

__Alterar a consulta__ para adicionar novas restrições de consulta que resolvam para restrições de índice existentes. Quando possível, preferir alterar a consulta a índices alterados.

+ [Saiba como ajustar o desempenho do query](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}

### Ajustar o índice

__Alterar (ou criar) um índice de AEM__ de forma que as restrições de consulta existentes sejam resolvidas para as atualizações de índice.

+ [Saiba como ajustar índices existentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}
+ [Saiba como criar índices](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target="_blank"}

## 3. Verificar{#verify}

Os ajustes feitos nos queries, índices ou ambos - devem ser verificados para garantir que reduzam os avisos de travessia.

![Explicar consulta](./assets/traversals/verify.gif)

Somente se [ajustes da consulta](#adjust-the-query) forem feitas, a consulta poderá ser testada diretamente em AEM as a Cloud Service por meio do Console do desenvolvedor [Explicar Consulta](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=pt-BR#queries){target="_blank"}. Explicar a consulta é executada em relação ao serviço de autor do AEM, no entanto, como as definições de índice são as mesmas nos serviços de Autor e Publicação, a validação de consultas em relação ao serviço de autor do AEM é suficiente.

If [ajustamentos ao índice](#adjust-the-index) forem feitas, o índice deverá ser implantado AEM as a Cloud Service. Com os ajustes de índice implantados, o Console do desenvolvedor [Explicar Consulta](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=pt-BR#queries){target="_blank"} pode ser usada para executar e ajustar mais a query.

Em última análise, todas as alterações (query e código) são confirmadas no Git e implantadas AEM as a Cloud Service usando o Cloud Manager. Depois de implantados, teste os caminhos de código associados aos avisos de travessia originais e verifique se os avisos de travessia não aparecem mais na variável `aemerror` log.

## Outros recursos

Confira esses outros recursos úteis para entender AEM índices, pesquisa e avisos de travessia.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Pesquisa e indexação" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Cloud 5 - Pesquisa e indexação"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Pesquisa e indexação">Cloud 5 - Pesquisa e indexação</a></p>
               <p class="is-size-6">A equipe do Cloud 5 mostra as explosões dos ins e outs da pesquisa e indexação AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=pt-BR" title="Pesquisa e indexação de conteúdo" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Pesquisa e indexação de conteúdo">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=pt-BR" title="Pesquisa e indexação de conteúdo">Documentação de pesquisa e indexação de conteúdo</a></p>
               <p class="is-size-6">Saiba como criar e gerenciar índices em AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=pt-BR" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernização de índices do Oak" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Modernização de índices do Oak">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernização de índices do Oak">Modernização de índices do Oak</a></p>
               <p class="is-size-6">Saiba como converter AEM 6 definições de índice Oak para serem AEM as a Cloud Service compatíveis e manter índices a partir de agora.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentação de definição de índice" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="Documentação de definição de índice">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentação de definição de índice">Documentação do índice Lucene</a></p>
               <p class="has-ellipsis is-size-6">O índice Apache Oak Jackrabbit Lucene que documenta todas as configurações de índice Lucene suportadas.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
