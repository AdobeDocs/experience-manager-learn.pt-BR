---
title: Outras ferramentas para depurar AEM SDK
description: Várias outras ferramentas podem ajudar na depuração da inicialização rápida local do SDK do AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 5%

---

# Outras ferramentas para depurar AEM SDK

Várias outras ferramentas podem ajudar na depuração do aplicativo na inicialização rápida local do SDK do AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

O CRXDE Lite é uma interface baseada na Web para interagir com o JCR, AEM repositório de dados. O CRXDE Lite oferece visibilidade total sobre o JCR, incluindo nós, propriedades, valores de propriedade e permissões.

CRXDE Lite está localizado em:

+ Ferramentas > Geral > CRXDE Lite
+ ou diretamente em [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Explicar consulta

![Explicar consulta](./assets/other-tools/explain-query.png)

Explique a ferramenta baseada na Web de Query AEM início rápido local do SDK, que fornece informações importantes sobre como o AEM interpreta e executa queries, e uma ferramenta valiosa para garantir que as consultas estejam sendo executadas de maneira eficiente pela AEM.

Explicar Consulta está localizado em:

+ Ferramentas > Diagnóstico > Desempenho da Consulta > Guia Explicar Consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)  > Guia Explicar consulta

## QueryBuilder Debugger

![QueryBuilder Debugger](./assets/other-tools/query-debugger.png)

O QueryBuilder Debugger é uma ferramenta baseada na Web que ajuda a depurar e entender consultas de pesquisa usando AEM sintaxe [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

O QueryBuilder Debugger está localizado em:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
