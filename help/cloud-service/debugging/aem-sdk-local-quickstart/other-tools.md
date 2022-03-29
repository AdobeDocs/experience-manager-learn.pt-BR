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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 2%

---

# Outras ferramentas para depurar AEM SDK

Várias outras ferramentas podem ajudar na depuração do aplicativo na inicialização rápida local do SDK do AEM.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

O CRXDE Lite é uma interface baseada na Web para interagir com o JCR, AEM repositório de dados. O CRXDE Lite oferece visibilidade total sobre o JCR, incluindo nós, propriedades, valores de propriedade e permissões.

CRXDE Lite está localizado em:

+ Ferramentas > Geral > CRXDE Lite
+ ou diretamente em [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Depuração de conteúdo

O CRXDE Lite fornece acesso direto ao JCR. O conteúdo visível por meio do CRXDE Lite é limitado pelas permissões concedidas ao usuário, o que significa que você não pode ver ou modificar tudo no JCR dependendo do seu acesso.

+ A estrutura do JCR é navegada e manipulada usando o painel de navegação esquerdo
+ Selecionar um nó no painel de navegação esquerdo expõe as propriedades do nó no painel inferior.
   + As propriedades podem ser adicionadas, removidas ou alteradas do painel
+ Clicar duas vezes em um nó de arquivo na navegação à esquerda abre o conteúdo do arquivo no painel superior direito
+ Toque no botão Salvar tudo, na parte superior esquerda, para continuar alterado, ou na seta para baixo, ao lado de Salvar tudo, para reverter quaisquer alterações não salvas.

![CRXDE Lite - Depuração de conteúdo](./assets/other-tools/crxde-lite__debugging-content.png)

Qualquer alteração feita diretamente no SDK AEM via CRXDE Lite pode ser difícil de rastrear e administrar. Conforme o caso, assegure-se de que as alterações feitas via CRXDE Lite retornem aos pacotes de conteúdo mutáveis do projeto AEM (`ui.content`) e comprometido com o Git. Idealmente, todas as alterações de conteúdo do aplicativo se originam da base de código e fluem para AEM SDK por meio de implantações, em vez de fazer alterações diretamente no SDK AEM por meio do CRXDE Lite.

### Depuração de controles de acesso

O CRXDE Lite fornece uma maneira de testar e avaliar o controle de acesso em um nó específico de um usuário ou grupo específico (também conhecido como principal).

Para acessar o console Controle de acesso de teste no CRXDE Lite, navegue até:

+ CRXDE Lite > Ferramentas > Testar controle de acesso ...

![CRXDE Lite - Testar controle de acesso](./assets/other-tools/crxde-lite__test-access-control.png)

1. Usando o campo Caminho , selecione um Caminho JCR para avaliar
1. Usando o campo Principal, selecione o usuário ou grupo para avaliar o caminho
1. Toque no botão Test

Os resultados são exibidos abaixo:

+ __Caminho__ reitera o caminho que foi avaliado
+ __Principal__ reitera o usuário ou grupo para o qual o caminho foi avaliado
+ __Princípios__ lista todos os principais dos quais o principal selecionado faz parte.
   + Isso é útil para entender as associações de grupo transitivas que podem fornecer permissões por herança
+ __Privilégios no Caminho__ lista todas as permissões do JCR que o principal selecionado tem no caminho avaliado

## Explicar consulta

![Explicar consulta](./assets/other-tools/explain-query.png)

Explique a ferramenta baseada na Web de Query AEM início rápido local do SDK, que fornece informações importantes sobre como o AEM interpreta e executa queries, e uma ferramenta valiosa para garantir que as consultas estejam sendo executadas de maneira eficiente pela AEM.

Explicar Consulta está localizado em:

+ Ferramentas > Diagnóstico > Desempenho da Consulta > Guia Explicar Consulta
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Guia Explicar Consulta

## QueryBuilder Debugger

![QueryBuilder Debugger](./assets/other-tools/query-debugger.png)

O QueryBuilder Debugger é uma ferramenta baseada na Web que ajuda a depurar e entender consultas de pesquisa usando AEM [QueryBuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) sintaxe.

O QueryBuilder Debugger está localizado em:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
