---
title: Gráficos de várias séries no AEM Forms
seo-title: Gráficos de várias séries no AEM Forms
description: Crie o Modelo de dados de formulário apropriado para criar gráficos de várias séries em documentos impressos e de canal da Web.
seo-description: Crie o Modelo de dados de formulário apropriado para criar gráficos de várias séries em documentos impressos e de canal da Web.
feature: Comunicação interativa
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# Gráficos de várias séries

O AEM Forms 6.5 apresentou a capacidade de criar e configurar gráficos de várias séries. Os gráficos de várias séries normalmente são usados em associação com o tipo de gráfico de Linha, Barra, Coluna. O gráfico a seguir é um bom exemplo de gráfico de várias séries. O gráfico mostra o crescimento de US$ 10.000,00 em 3 diferentes fundos mutualistas durante um período de tempo. Para criar e usar gráficos desse tipo no AEM Forms, será necessário criar o Modelo de dados de formulário apropriado.

![multisérie](assets/seriescharts.jfif)

Para criar gráficos de várias séries no AEM Forms, é necessário criar um Modelo de dados de formulário apropriado com entidades e associações necessárias entre as entidades. A captura de tela a seguir destaca as entidades e as associações entre as três entidades. No nível superior, temos uma entidade chamada &quot;Organização&quot;, que tem uma associação de um para muitos com a entidade do Fundo. A entidade Fundo, por sua vez, tem uma associação de um para muitos com a entidade Desempenho.

![formdatamodel](assets/formdatamodel.jfif)


## Criar modelo de dados de formulário para gráficos de várias séries

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurar Gráficos de Série de Linhas

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Para testar isso em seu sistema, siga as seguintes etapas

* [Baixe e importe o MutualFundFactSheet.zip usando o Gerenciador de Pacotes do AEM.](assets/mutualfundfactsheet.zip)
* [Baixe o SeriesChartSampleData.json no seu disco rígido.](assets/serieschartsampledata.json) Esses são os dados de amostra que serão usados para preencher o gráfico.
* [Navegue até Formulários e Documentos.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Com cuidado, selecione o modelo de comunicações interativas &quot;MútuoFundoCrescimentoFactSheet&quot;.
* Clique em Visualizar | Fazer upload de dados de amostra.
* Navegue até o arquivo de dados de amostra fornecido como parte deste artigo.
* Visualize o canal de impressão da comunicação interativa &quot;MútuoFundoCrescimentoFactSheet&quot; com os dados de amostra baixados na etapa anterior.
