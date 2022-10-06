---
title: Gráficos de várias séries no AEM Forms
description: Crie o Modelo de dados de formulário apropriado para criar gráficos de várias séries em documentos impressos e de canal da Web.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Gráficos de várias séries

O AEM Forms 6.5 apresentou a capacidade de criar e configurar gráficos de várias séries. Os gráficos de várias séries normalmente são usados em associação com o tipo de gráfico de Linha, Barra, Coluna. O gráfico a seguir é um bom exemplo de gráfico de várias séries. O gráfico mostra o crescimento de US$ 10.000,00 em 3 diferentes fundos mutualistas durante um período de tempo. Para criar e usar gráficos desse tipo no AEM Forms, será necessário criar o Modelo de dados de formulário apropriado.

![Gráfico de várias séries](assets/seriescharts.jfif)

Para criar gráficos de várias séries no AEM Forms, é necessário criar um Modelo de dados de formulário apropriado com entidades e associações necessárias entre as entidades. A captura de tela a seguir destaca as entidades e as associações entre as três entidades. No nível superior, temos uma entidade chamada &quot;Organização&quot;, que tem uma associação de um para muitos com a entidade do Fundo. A entidade Fundo, por sua vez, tem uma associação de um para muitos com a entidade Desempenho.

![Modelo de dados do formulário](assets/formdatamodel.jfif)

## Criar modelo de dados de formulário para gráficos de várias séries

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)

### Configurar Gráficos de Série de Linhas

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)

Para testar isso em seu sistema, siga as seguintes etapas

* [Baixe e importe o CollectFundFactSheet.zip usando AEM Gerenciador de Pacotes.](assets/mutualfundfactsheet.zip)
* [Baixe o SeriesChartSampleData.json no seu disco rígido.](assets/serieschartsampledata.json) Esses são os dados de amostra usados para preencher o gráfico.
* [Navegue até Forms e Documentos.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Com cuidado, selecione o modelo de comunicações interativas &quot;MútuoFundoCrescimentoFactSheet&quot;.
* Clique em Visualizar | Canal de impressão | Fazer upload de dados de amostra.
* Navegue até o arquivo de dados de amostra fornecido como parte deste artigo.
* Visualize o canal de impressão da comunicação interativa &quot;MútuoFundoCrescimentoFactSheet&quot; com os dados de amostra baixados na etapa anterior.
