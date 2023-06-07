---
title: Avisos transversais no AEM as a Cloud Service
description: Saiba como atenuar avisos de passagem no AEM as a Cloud Service.
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
source-git-commit: 678ecb99b1e63b9db6c9668adee774f33b2eefab
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 10%

---

# Avisos transversais

>[!TIP]
>Marque esta página como favorita para referência futura.

_O que são avisos de travessia?_

Os avisos transversais são __aemerror__ instruções de log indicando consultas com baixo desempenho estão sendo executadas no serviço de publicação do AEM. Os avisos transversais normalmente se manifestam no AEM de duas maneiras:

1. __Consultas lentas__ que não usam índices, resultando em tempos de resposta lentos.
1. __Consultas com falha__, que lançam um `RuntimeNodeTraversalException`, resultando em uma experiência com falha.

Permitir que avisos de passagem sejam desmarcados reduz o desempenho do AEM e pode resultar em experiências quebradas para os usuários.

## Como resolver avisos de travessia

A mitigação de avisos de travessia pode ser abordada usando três etapas simples: analisar, ajustar e verificar. Espere várias iterações de ajuste e verifique antes de identificar os ajustes ideais.

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
                <p class="headline is-size-5 has-text-weight-bold">Analisar o problema</p>
               <p class="is-size-6">Identificar e entender quais consultas estão percorrendo.</p>
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
               <p class="is-size-6">Atualize consultas e índices para evitar percursos de consulta.</p>
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
               <p class="is-size-6">Verifique se as alterações nas consultas e nos índices removem os percursos.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verificar</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. Analisar{#analyze}

Primeiro, identifique quais serviços de publicação do AEM estão exibindo avisos de passagem. Para fazer isso, no Cloud Manager, [baixar Serviços de publicação&#39; `aemerror` logs](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target="_blank"} de todos os ambientes (Desenvolvimento, Preparo e Produção) para o passado __três dias__.

![Baixar logs as a Cloud Service do AEM](./assets/traversals/download-logs.jpg)

Abra os arquivos de registro e procure a classe Java™ `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. O log que contém avisos de passagem contém uma série de instruções semelhantes a:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

Dependendo do contexto da execução da consulta, as instruções de log podem conter informações úteis sobre o originador da consulta:

+ URL de solicitação HTTP associado à execução da consulta

   + Exemplo: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Sintaxe de consulta do Oak

   + Exemplo: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ Consulta XPath

   + Exemplo: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Código que executa a consulta

   + Exemplo:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Consultas com falha__ são seguidos de um `RuntimeNodeTraversalException` semelhante a:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. Ajustar{#adjust}

Quando as queries ofensivas e seu código de chamada forem descobertos, os ajustes deverão ser feitos. Dois tipos de ajustes podem ser feitos para atenuar avisos de travessia:

### Ajustar a consulta

__Alterar a consulta__ para adicionar novas restrições de consulta que resolvem restrições de índice existentes. Quando possível, prefira alterar a consulta a alterar os índices.

+ [Saiba como ajustar o desempenho da consulta](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}

### Ajustar o índice

__Alterar (ou criar) um índice AEM__ de forma que as restrições de consulta existentes possam ser resolvidas para as atualizações de índice.

+ [Saiba como ajustar índices existentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target="_blank"}
+ [Saiba como criar índices](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target="_blank"}

## 3. Verificar{#verify}

Ajustes feitos em consultas, índices ou ambos - devem ser verificados para garantir que atendam aos avisos de passagem.

![Explicar consulta](./assets/traversals/verify.gif)

Se somente [ajustes na consulta](#adjust-the-query) feita, a consulta pode ser testada diretamente no AEM as a Cloud Service por meio do Console do desenvolvedor [Explicar consulta](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=pt-BR#queries){target="_blank"}. A Consulta de explicação é executada em relação ao serviço do Autor do AEM, no entanto, como as definições de índice são as mesmas nos serviços do Autor e de Publicação, validar as consultas no serviço do Autor do AEM é suficiente.

Se [ajustes no índice](#adjust-the-index) forem feitas, o índice deve ser implantado no AEM as a Cloud Service. Com os ajustes de índice implantados, o Console do desenvolvedor [Explicar consulta](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=pt-BR#queries){target="_blank"} pode ser usado para executar e ajustar ainda mais a consulta.

Por fim, todas as alterações (consulta e código) são confirmadas no Git e implantadas no AEM as a Cloud Service usando o Cloud Manager. Depois de implantados, testam novamente os caminhos de código associados aos avisos de passagem originais e verificam se os avisos de passagem não aparecem mais no `aemerror` registro.

## Outros recursos

Confira estes outros recursos úteis para entender os índices AEM, pesquisa e avisos de passagem.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Pesquisa e indexação" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Cloud 5 - Pesquisa e indexação"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5 - Pesquisa e indexação">Cloud 5 - Pesquisa e indexação</a></p>
               <p class="is-size-6">A equipe da Cloud 5 mostra como explorar os detalhes da pesquisa e indexação no AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
               <p class="is-size-6">Saiba como criar e gerenciar índices no AEM as a Cloud Service.</p>
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
               <p class="is-size-6">Saiba como converter definições de índice AEM 6 Oak para serem compatíveis com AEM as a Cloud Service e manter índices daqui para frente.</p>
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
               <p class="has-ellipsis is-size-6">A referência de índice Apache Oak Jackrabbit Lucene que documenta todas as configurações de índice Lucene compatíveis.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
