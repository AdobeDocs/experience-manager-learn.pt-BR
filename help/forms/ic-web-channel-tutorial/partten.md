---
title: Configuração do Painel de Gravação do Outlook
seo-title: Configuração do Painel de Gravação do Outlook
description: Esta é a parte 10 de um tutorial de várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, iremos configurar o Painel Retirada do Outlook adicionando componentes de texto e gráfico.
seo-description: Esta é a parte 10 de um tutorial de várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, iremos configurar o Painel Retirada do Outlook adicionando componentes de texto e gráfico.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
translation-type: tm+mt
source-git-commit: f6a9c0522219614b87c483504c9c64875f2e4286
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# Configurando o Painel de Gravação do Outlook{#configuring-retirement-outlook-panel}

* Esta é a parte 10 de um tutorial de várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, iremos configurar o Painel Retirada do Outlook adicionando componentes de texto e gráfico.

* Faça logon no AEM Forms e navegue até Adobe Experience Manager > Forms > Forms e Documentos.

* Abra a pasta 401KStatement.

* Abra o documento 401KStatement no modo de edição.

**Configurar a área de público alvo do LeftPanel**

* Toque na área do público alvo LeftPanel no lado direito e clique no ícone &quot;+&quot; para abrir a caixa de diálogo Inserir componente.

* Componente Inserir texto.

* Toque com cuidado no componente de texto recém-adicionado para exibir a barra de ferramentas do componente

* Selecione o ícone &quot;lápis&quot; para editar o texto padrão.

* Substitua o texto padrão por &quot;**Seu Outlook de Renda de Baixa&quot;**

**Configurar a área de público alvo do Painel direito**

* Toque na área público alvo RightPanel no lado direito e clique no ícone &quot;+&quot; para abrir a caixa de diálogo Inserir componente.

* Componente Inserir texto.

* Toque com cuidado no componente de texto recém-adicionado para exibir a barra de ferramentas do componente.

* Selecione o ícone &quot;lápis&quot; para editar o texto padrão.

* Substitua o texto padrão por &quot;**Renda de Baixa Mensal Estimada&quot;**

## Adicionar Fragmento de Documento do Outlook de Renda {#add-retirement-income-outlook-document-fragment}

* Clique no ícone Ativos e aplique o filtro para exibir ativos do tipo &quot;Fragmentos de Documento&quot;. Arraste e solte o fragmento de documento RetimentaçãoRendaOutlook na área de público alvo do painel esquerdo.

* Você pode consultar [esta página](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html) ao adicionar fragmento de documento a áreas de conteúdo.

## Adicionar gráfico de rendimento mensal estimado {#adding-estimated-monthly-income-chart}

* Clique na área público alvo RightPanel no lado direito. Clique no ícone &quot;+&quot; para inserir o componente gráfico. Usaremos um gráfico de colunas para exibir a renda mensal estimada. Toque com cuidado no componente de gráfico recém-inserido. Selecione o ícone &quot;chave inglesa&quot; para abrir a folha de propriedades de configuração.Configure o gráfico com as seguintes propriedades, conforme mostrado na captura de tela abaixo.

**AEM Forms 6.4 - Configurando o Gráfico de Coluna de Renda Mensal Estimada**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configurando o Gráfico de Coluna de Renda Mensal Estimada**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




