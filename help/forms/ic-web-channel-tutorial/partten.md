---
title: Configurando o Painel do Outlook de Baixa
description: Esta é a parte 10 de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, configuraremos o Painel do Outlook de aposentadoria adicionando componentes de texto e gráfico.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
duration: 82
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# Configurando o Painel do Outlook de Baixa{#configuring-retirement-outlook-panel}

* Esta é a parte 10 de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, configuraremos o Painel do Outlook de aposentadoria adicionando componentes de texto e gráfico.

* Faça logon no AEM Forms e navegue até Adobe Experience Manager > Forms > Forms e documentos.

* Abra a pasta 401KStatement.

* Abra o documento 401KStatement no modo de edição.

**Configurar área de destino do LeftPanel**

* Toque na área de destino do Painel esquerdo no lado direito e clique no ícone &quot;+&quot; para exibir a caixa de diálogo Inserir componente.

* Componente Inserir texto.

* Toque com cuidado no componente de texto recém-adicionado para mostrar a barra de ferramentas do componente

* Selecione o ícone de &quot;lápis&quot; para editar o texto padrão.

* Substituir o texto padrão por &quot;**Seu Outlook de Renda de Aposentadoria&quot;**

**Configurar área de destino do RightPanel**

* Toque na área de destino do RightPanel no lado direito e clique no ícone &quot;+&quot; para abrir a caixa de diálogo Inserir componente.

* Componente Inserir texto.

* Toque com cuidado no componente de texto recém-adicionado para mostrar a barra de ferramentas do componente.

* Selecione o ícone de &quot;lápis&quot; para editar o texto padrão.

* Substituir o texto padrão por &quot;**Receita Mensal Estimada da Aposentadoria&quot;**

## Adicionar Fragmento de Documento do Outlook de Renda de Baixa {#add-retirement-income-outlook-document-fragment}

* Clique no ícone Ativos e aplique o filtro para exibir ativos do tipo &quot;Fragmentos de documento&quot;. Arraste e solte o fragmento do documento RetiradaReceitaOutlook na área de destino do Painel esquerdo.

* Você pode consultar [nesta página](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) sobre a adição do fragmento do documento às áreas de conteúdo.

## Adição do Gráfico de Receita Mensal Estimada {#adding-estimated-monthly-income-chart}

* Clique na área de destino do RightPanel no lado direito. Clique no ícone &quot;+&quot; para inserir o componente de gráfico. Usaremos um gráfico de colunas para exibir a receita mensal estimada. Toque com cuidado no componente de gráfico recém-inserido. Selecione o ícone &quot;Chave inglesa&quot; para abrir a folha de propriedades de configuração.Configure o gráfico com as seguintes propriedades, conforme mostrado na captura de tela abaixo.

**AEM Forms 6.4 - Configuração do Gráfico de Colunas de Receita Mensal Estimada**

![formulário64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configuração do gráfico de colunas de receita mensal estimada**

![formulários65](assets/estimatedmonthlyincomechart65.PNG)

## Próximas etapas

[Configurar gráfico de pizza](./parteleven.md)
