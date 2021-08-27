---
title: Configuração do Painel do Outlook de Baixa
seo-title: Configuring Retirement Outlook Panel
description: Esta é a parte 10 de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, vamos configurar o Painel de Redimensionamento do Outlook adicionando componentes de texto e gráfico.
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
source-git-commit: 0049c9fd864bd4dd4f8c33b1e40e94aad3ffc5b9
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# Configuração do Painel do Outlook de Baixa{#configuring-retirement-outlook-panel}

* Esta é a parte 10 de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, vamos configurar o Painel de Redimensionamento do Outlook adicionando componentes de texto e gráfico.

* Faça logon no AEM Forms e navegue até Adobe Experience Manager > Forms > Forms &amp; Documents.

* Abra a pasta 401KStatement .

* Abra o documento 401KStatement no modo de edição.

**Configurar área de destino do Painel esquerdo**

* Toque na área de destino do Painel esquerdo no lado direito e clique no ícone &quot;+&quot; para abrir a caixa de diálogo Inserir componente.

* Inserir componente de Texto.

* Toque com cuidado no componente de texto recém-adicionado para abrir a barra de ferramentas do componente

* Selecione o ícone de &quot;lápis&quot; para editar o texto padrão.

* Substitua o texto padrão por &quot;**O seu Outlook de Renda de Baixa&quot;**

**Configurar área de destino do Painel direito**

* Toque na área de destino do Painel direito no lado direito e clique no ícone &quot;+&quot; para abrir a caixa de diálogo Inserir componente.

* Inserir componente de Texto.

* Toque com cuidado no componente de texto recém-adicionado para abrir a barra de ferramentas do componente.

* Selecione o ícone de &quot;lápis&quot; para editar o texto padrão.

* Substitua o texto padrão por &quot;**Receita de Baixa Mensal Estimada&quot;**

## Adicionar Fragmento do Documento do Outlook de Renda de Baixa {#add-retirement-income-outlook-document-fragment}

* Clique no ícone Ativos e aplique o filtro para exibir ativos do tipo &quot;Fragmentos do documento&quot;. Arraste e solte o fragmento de documento RetimentaçãoRendaOutlook na área de destino do Painel esquerdo.

* Você pode consultar [esta página](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) sobre como adicionar fragmento de documento a áreas de conteúdo.

## Adição de Gráfico de Receita Mensal Estimada {#adding-estimated-monthly-income-chart}

* Clique na área de destino do Painel direito no lado direito. Clique no ícone &quot;+&quot; para inserir o componente de gráfico. Usaremos um gráfico de coluna para exibir a receita mensal estimada. Toque com cuidado no componente de gráfico recém-inserido. Selecione o ícone &quot;Chave&quot; para abrir a folha de propriedades de configuração.Configure o gráfico com as seguintes propriedades, conforme mostrado na captura de tela abaixo.

**AEM Forms 6.4 - Configuração do Gráfico da Coluna de Renda Mensal Estimada**

![formulário64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configuração do Gráfico da Coluna de Renda Mensal Estimada**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




