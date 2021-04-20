---
title: Configuração do Painel de Combinação de Investimentos
seo-title: Configuração do Painel de Combinação de Investimentos
description: Esta é a parte 11 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, adicionaremos gráficos de pizza para exibir o mix de investimento atual e de modelo.
seo-description: Esta é a parte 11 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, adicionaremos gráficos de pizza para exibir o mix de investimento atual e de modelo.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 1%

---


# Configuração do Painel de Combinação de Investimentos

Nesta parte, adicionaremos gráficos de pizza para exibir a combinação de investimento atual e modelo.

* Faça logon no AEM Forms e navegue até Adobe Experience Manager > Formulários > Formulários e documentos.

* Abra a pasta 401KStatement .

* Abra a declaração 401KS no modo de edição.

* Acrescentaremos dois gráficos de pizza para representar a combinação de investimento atual e modelo do titular da conta.

## Combinação de ativos atuais {#current-asset-mix}

* Toque no painel &quot;CurrentAssetMix&quot; no lado direito e selecione o ícone &quot;+&quot; e insira o componente de texto. Altere o texto padrão para &quot;Combinação de ativos atuais&quot;.

* Toque no painel &quot;CurrentAssetMix&quot; e selecione o ícone &quot;+&quot; e insira o componente de gráfico. Toque no componente de gráfico recém-inserido e clique no ícone &quot;chave inglesa&quot; para abrir a folha de propriedades de configuração do gráfico.

* Defina as propriedades conforme mostrado na imagem abaixo. Certifique-se de que o tipo do gráfico é Pizza.

* Observe o Objeto de Modelo de Dados vinculado aos eixos X e Y. Você precisa selecionar o elemento raiz do modelo de dados de formulário e, em seguida, detalhar para selecionar o elemento apropriado.

* ![currentassetmix](assets/currentassetmixchart.png)

## Mistura de ativos de modelo {#model-asset-mix}

* Toque no painel &quot;RecommendedAssetMix&quot; no lado direito e selecione o ícone &quot;+&quot; e insira o componente de texto. Altere o texto padrão para &quot;Model Asset Mix&quot;.

* Toque no painel &quot;RecommendedAssetMix&quot; e selecione o ícone &quot;+&quot; e insira o componente de gráfico. Toque no componente de gráfico recém-inserido e clique no ícone &quot;chave inglesa&quot; para abrir a folha de propriedades de configuração do gráfico.

* Defina as propriedades conforme mostrado na imagem abaixo. Certifique-se de que o tipo do gráfico é Pizza.

* Observe o Objeto de Modelo de Dados vinculado aos eixos X e Y. Você precisa selecionar o elemento raiz do modelo de dados de formulário e, em seguida, detalhar para selecionar o elemento apropriado.

* ![assettype](assets/modelassettypechart.png)

