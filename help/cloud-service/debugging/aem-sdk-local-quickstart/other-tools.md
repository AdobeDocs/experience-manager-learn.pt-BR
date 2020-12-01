---
title: Outras ferramentas para depurar AEM SDK
description: Várias outras ferramentas podem ajudar na depuração do AEM Início rápido local do SDK.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 3%

---


# Outras ferramentas para depurar AEM SDK

Uma variedade de outras ferramentas pode ajudar na depuração do aplicativo no início rápido local do SDK do AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

O CRXDE Lite é uma interface baseada na Web para interagir com o JCR, AEM repositório de dados. O CRXDE Lite oferece total visibilidade sobre o JCR, incluindo nós, propriedades, valores de propriedade e permissões.

O CRXDE Lite está localizado em:

+ Ferramentas > Geral > CRXDE Lite
+ ou diretamente em [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Explicar consulta

![Explicar consulta](./assets/other-tools/explain-query.png)

Explique a ferramenta baseada na Web do Query AEM início rápido local do SDK, que fornece insights importantes sobre como AEM interpreta e executa query, e uma ferramenta inestimável para garantir que os query estejam sendo executados de maneira eficiente pela AEM.

O Query de explicação está localizado em:

+ Ferramentas > Diagnóstico > Desempenho do Query > Guia Explicar o Query
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Guia Explicar Query

## Depurador do QueryBuilder

![Depurador do QueryBuilder](./assets/other-tools/query-debugger.png)

O depurador do QueryBuilder é uma ferramenta baseada na Web que ajuda a depurar e entender query de pesquisa usando a sintaxe AEM [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

O QueryBuilder Debugger está localizado em:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer e AEM plug-in do Chrome

![Sling Log Tracer e AEM plug-in do Chrome](./assets/other-tools/log-tracer.png)

[O Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html), que é fornecido com AEM início rápido local do SDK, permite o rastreamento detalhado das solicitações HTTP, expondo informações aprofundadas de depuração por solicitação. A configuração [Log Tracer OSGi deve ser configurada](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1) para habilitar esse recurso.

O plug-in de código aberto [AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US) para o [navegador da Web Google Chrome](https://www.google.com/chrome/), integra-se ao Log Tracer, expondo as informações de depuração diretamente nas Ferramentas de Desenvolvimento do Chrome.

_O plug-in AEM Chrome é uma ferramenta de código aberto e o Adobe não oferece suporte para ele._

