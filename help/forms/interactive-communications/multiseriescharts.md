---
title: Gráficos de várias séries no AEM Forms
description: Crie o modelo de dados de formulário apropriado para criar gráficos de várias séries em documentos impressos e de canal da Web.
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
duration: 430
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Gráficos Multissérie

O AEM Forms 6.5 apresentou a capacidade de criar e configurar vários gráficos de série. Normalmente, os gráficos de várias séries são usados em associação com o tipo de gráfico Linha, Barra e Coluna. O gráfico a seguir é um bom exemplo de gráfico de várias séries. O gráfico mostra o crescimento de US$ 10.000,00 em três fundos mútuos diferentes ao longo de um período. Para criar e usar gráficos desse tipo no AEM Forms, será necessário criar o modelo de dados de formulário apropriado.

![Gráfico de várias séries](assets/series_charts.png)

Para criar gráficos de várias séries no AEM Forms, você precisa criar um Modelo de dados de formulário apropriado com as entidades necessárias e associações entre as entidades. A captura de tela a seguir destaca as entidades e as associações entre as 3 entidades. No nível superior, temos uma entidade chamada &quot;Organização&quot;, que tem uma associação um para muitos com a entidade Fundo. A entidade Fundo, por sua vez, tem uma associação um para muitos com a entidade Desempenho.

![Modelo de dados do formulário](assets/form_data_model.png)

## Criar Modelo de Dados de Formulário para Gráficos Multissérie

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### Configurar Gráficos de Série de Linha

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

Para testar isso em seu sistema, siga as etapas a seguir

* [Baixe e importe o MutualFundFactSheet.zip usando o Gerenciador de pacotes do AEM.](assets/mutualfundfactsheet.zip)
* [Baixe o SeriesChartSampleData.json no disco rígido.](assets/serieschartsampledata.json) Estes são os dados de exemplo usados para preencher o gráfico.
* [Navegue até Forms e Documentos.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Selecione cuidadosamente o modelo de comunicações interativas &quot;MutualFundGrowthFactSheet&quot;.
* Clique em Visualizar | Canal de impressão | Carregar dados de amostra.
* Navegue até o arquivo de dados de amostra fornecido como parte deste artigo.
* Pré-visualize o canal de impressão da comunicação interativa &quot;MutualFundGrowthFactSheet&quot; com os dados de amostra baixados na etapa anterior.
