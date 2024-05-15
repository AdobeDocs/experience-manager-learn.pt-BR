---
title: Configuração do painel de combinação de investimentos
description: Esta é a parte 11 do tutorial em várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, adicionaremos gráficos de pizza para exibir a combinação de investimento atual e modelo.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 69
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# Configuração do painel de combinação de investimentos

Nesta parte, adicionaremos gráficos de pizza para exibir a combinação de investimento atual e modelo.

* Faça logon no AEM Forms e navegue até Adobe Experience Manager > Forms > Forms e documentos.

* Abra a pasta 401KStatement.

* Abra a instrução 401KS no modo de edição.

* Vamos adicionar dois gráficos de pizza para representar a combinação de investimento atual e modelo do titular da conta.

## Combinação de ativos atual {#current-asset-mix}

* Toque no painel &quot;CurrentAssetMix&quot; no lado direito e selecione o ícone &quot;+&quot; e insira o componente de texto. Altere o texto padrão para &quot;Combinação de ativos atual&quot;.

* Toque no painel &quot;CurrentAssetMix&quot;, selecione o ícone &quot;+&quot; e insira o componente de gráfico. Toque no componente de gráfico recém-inserido e clique no ícone &quot;chave inglesa&quot; para abrir a folha de propriedades de configuração do gráfico.

* Defina as propriedades conforme mostrado na imagem abaixo. Verifique se o tipo de gráfico é de Pizza.

* Observe o objeto de modelo de dados vinculado aos eixos X e Y. É necessário selecionar o elemento raiz do modelo de dados de formulário e detalhar para selecionar o elemento apropriado.

* ![currentassetmix](assets/currentassetmixchart.png)

## Combinação de ativos do modelo {#model-asset-mix}

* Toque no painel &quot;RecommendedAssetMix&quot; no lado direito e selecione o ícone &quot;+&quot; e insira o componente de texto. Altere o texto padrão para &quot;Combinação de ativos do modelo&quot;.

* Toque no painel &quot;RecommendedAssetMix&quot; e selecione o ícone &quot;+&quot; e insira o componente de gráfico. Toque no componente de gráfico recém-inserido e clique no ícone &quot;chave inglesa&quot; para abrir a folha de propriedades de configuração do gráfico.

* Defina as propriedades conforme mostrado na imagem abaixo. Verifique se o tipo de gráfico é de Pizza.

* Observe o objeto de modelo de dados vinculado aos eixos X e Y. É necessário selecionar o elemento raiz do modelo de dados de formulário e detalhar para selecionar o elemento apropriado.

* ![assettype](assets/modelassettypechart.png)

## Próximas etapas

[Preparar para entregar o documento de canal da Web](./parttwelve.md)
