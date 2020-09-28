---
title: Gráficos de várias séries no AEM Forms
seo-title: Gráficos de várias séries no AEM Forms
description: Crie um Modelo de dados de formulário apropriado para criar gráficos de várias séries em documentos de canais da Web e impressos.
seo-description: Crie um Modelo de dados de formulário apropriado para criar gráficos de várias séries em documentos de canais da Web e impressos.
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Gráficos de várias séries

O AEM Forms 6.5 introduziu a capacidade de criar e configurar vários gráficos de série. Os gráficos de várias séries normalmente são usados em associação com o tipo de gráfico Linha, Barra e Coluna. O gráfico a seguir é um bom exemplo de gráfico de várias séries. O gráfico mostra o crescimento de $10.000 dólares em 3 diferentes fundos mútuos ao longo de um período de tempo. Para poder criar e usar gráficos desse tipo no AEM Forms, será necessário criar o Modelo de dados de formulário apropriado.

![várias séries](assets/seriescharts.jfif)

Para criar gráficos de várias séries no AEM Forms, é necessário criar um Modelo de dados de formulário apropriado com entidades e associações necessárias entre as entidades. A captura de tela a seguir destaca as entidades e as associações entre as 3 entidades. No nível superior, temos uma entidade chamada &quot;Organização&quot;, que tem uma associação de um para muitos com a entidade do Fundo. A entidade do Fundo, por sua vez, tem uma associação um-para-muitos com a entidade Performance.

![formdatamodel](assets/formdatamodel.jfif)


## Criar modelo de dados de formulário para gráficos de várias séries

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurar Gráficos de Séries de Linha

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Para testar isso em seu sistema, siga as seguintes etapas

* [Baixe e importe o CollectiveFundFactSheet.zip usando o AEM Package Manager.](assets/mutualfundfactsheet.zip)
* [Baixe SeriesChartSampleData.json no disco rígido.](assets/serieschartsampledata.json) Estes são os dados de amostra que serão usados para preencher o gráfico.
* [Navegue até Forms e Documentos.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Selecione com cuidado o modelo de comunicação interativa &quot;CollectivateFundGrowthFactSheet&quot;.
* Clique na Pré-visualização | Carregar dados de amostra.
* Navegue até o arquivo de dados de amostra fornecido como parte deste artigo.
* Pré-visualização o canal de impressão da comunicação interativa &quot;CollectiveFundGrowthFactSheet&quot; com os dados de amostra baixados na etapa anterior.
