---
title: Analisar dados com o Analysis Workspace
description: Saiba como mapear dados capturados de um site do Adobe Experience Manager para métricas e dimensões em conjuntos de relatórios do Adobe Analytics. Saiba como criar um painel de relatórios detalhado usando o recurso Analysis Workspace do Adobe Analytics.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
jira: KT-6409
thumbnail: KT-6296.jpg
doc-type: Tutorial
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
badgeIntegration: label="Integração" type="positive"
last-substantial-update: 2022-06-15T00:00:00Z
duration: 443
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2072'
ht-degree: 0%

---

# Analisar dados com o Analysis Workspace

Saiba como mapear dados capturados de um site do Adobe Experience Manager para métricas e dimensões em conjuntos de relatórios do Adobe Analytics. Saiba como criar um painel de relatórios detalhado usando o recurso Analysis Workspace do Adobe Analytics.

## O que você vai criar {#what-build}

A equipe de marketing da WKND está interessada em saber quais botões do `Call to Action (CTA)` estão tendo o melhor desempenho na página inicial. Neste tutorial, crie um projeto no **Analysis Workspace** para visualizar o desempenho de diferentes botões do CTA e entender o comportamento do usuário no site. As informações a seguir são capturadas usando o Adobe Analytics quando um usuário clica em um botão de Chamada para ação (CTA) na página inicial da WKND.

**Variáveis do Analytics**

Abaixo estão as variáveis do Analytics que estão sendo rastreadas:

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA Click Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Objetivos {#objective}

1. Crie um conjunto de relatórios ou use um existente.
1. Configure [Variáveis de conversão (eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html) e [Eventos de sucesso (Eventos)](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event) no Conjunto de relatórios.
1. Crie um [projeto do Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html) para analisar dados com a ajuda de ferramentas que permitem compilar, analisar e compartilhar insights rapidamente.
1. Compartilhar o projeto do Analysis Workspace com outros membros da equipe.

## Pré-requisitos

Este tutorial é uma continuação do [Rastrear componente clicado com o Adobe Analytics](./track-clicked-component.md) e presume que você tenha:

* Uma **Propriedade de Marca** com a [extensão do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) habilitada
* Servidor de rastreamento e ID do conjunto de relatórios de teste/desenvolvimento do **Adobe Analytics**. Consulte a documentação a seguir para [criar um conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* Extensão do navegador do [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) configurada com uma propriedade de marca carregada no [Site WKND](https://wknd.site/us/en.html) ou em um site AEM com a Camada de Dados do Adobe habilitada.

## Variáveis de conversão (eVars) e Eventos bem-sucedidos (evento)

A variável de conversão do Custom Insight (ou eVar) é colocada no código Adobe nas páginas da Web selecionadas do site. Seu objetivo principal é segmentar as métricas de sucesso de conversão em relatórios de marketing personalizados. Uma eVar pode ser baseada em visitas e funciona de forma semelhante aos cookies. Os valores transmitidos para as variáveis do eVar seguem o usuário por um período predeterminado.

Quando uma eVar é definida como o valor de um visitante, a Adobe lembra automaticamente desse valor até sua expiração. Quaisquer eventos bem-sucedidos que um visitante encontrar enquanto o valor do eVar estiver ativo serão contados em relação ao valor do eVar.

As eVars são usadas com mais eficiência para medir causa e efeito, como:

* Quais campanhas internas influenciaram a receita
* Quais anúncios de banner resultaram em um registro
* O número de vezes que uma pesquisa interna foi usada antes de fazer um pedido

Eventos bem-sucedidos são ações que podem ser rastreadas. Você determina o que é um evento bem-sucedido. Por exemplo, se um visitante clicar em um botão do CTA, o evento de clique pode ser considerado um evento bem-sucedido.

### Configurar eVars

1. Na página inicial do Adobe Experience Cloud, selecione sua organização e inicie o Adobe Analytics.

   ![AEP do Analytics](assets/create-analytics-workspace/analytics-aep.png)

1. Na barra de ferramentas do Analytics, clique em **Admin** > **Conjuntos de relatórios** e localize seu Conjunto de relatórios.

   ![Conjunto de relatórios do Analytics](assets/create-analytics-workspace/select-report-suite.png)

1. Selecione o Conjunto de relatórios > **Editar configurações** > **Conversão** > **Variáveis de conversão**

   ![Variáveis de conversão do Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. Usando a opção **Adicionar novo**, vamos criar Variáveis de conversão para mapear o esquema conforme abaixo:

   * `eVar5` - `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Adicionar novas eVars](assets/create-analytics-workspace/add-new-evars.png)

1. Forneça um nome e uma descrição apropriados para cada eVars e **Salve** suas alterações. No projeto do Analysis Workspace, as eVars com nome apropriado são usadas, portanto, um nome amigável torna as variáveis facilmente detectáveis.

   ![eVars](assets/create-analytics-workspace/evars.png)

### Configurar eventos bem-sucedidos

Em seguida, vamos criar um evento para rastrear os cliques do botão CTA.

1. Na janela **Gerenciador de Conjunto de Relatórios**, selecione a **Id de Conjunto de Relatórios** e clique em **Editar Configurações**.
1. Clique em **Conversão** > **Eventos bem-sucedidos**
1. Usando a opção **Adicionar novo**, crie um evento bem-sucedido personalizado para rastrear o clique no Botão do CTA e **Salve** suas alterações.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVars](assets/create-analytics-workspace/add-success-event.png)

## Criar um projeto no Analysis Workspace {#workspace-project}

O Analysis Workspace é uma ferramenta de navegador flexível que permite criar análises e compartilhar insights rapidamente. Usando a interface de arrastar e soltar, você pode criar a análise, adicionar visualizações para dar vida aos dados, preparar um conjunto de dados, compartilhar e agendar projetos com qualquer pessoa em sua organização.

Em seguida, crie um [projeto](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html#analysis-workspace) para criar um painel e analisar o desempenho dos botões do CTA em todo o site.

1. Na barra de ferramentas do Analytics, selecione **Workspace** e clique para **Criar um Novo Projeto**.

   ![Workspace](assets/create-analytics-workspace/create-workspace.png)

1. Escolha começar a partir de um **projeto em branco** ou selecione um dos modelos pré-criados, fornecidos pela Adobe ou modelos personalizados criados por sua organização. Vários modelos estão disponíveis, dependendo da análise ou caso de uso que você tenha em mente. [Saiba mais](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) sobre as diferentes opções de modelo disponíveis.

   No projeto do Workspace, painéis, tabelas, visualizações e componentes são acessados pelo painel esquerdo. Eles formam os blocos de construção do seu projeto.

   * **[Componentes](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** - Componentes são dimensões, métricas, segmentos ou intervalos de datas que podem ser combinados em uma Tabela de forma livre para começar a responder às suas perguntas comerciais. Familiarize-se com cada tipo de componente antes de mergulhar na análise. Depois de dominar a terminologia do componente, você pode começar a arrastar e soltar para criar a análise em uma tabela de forma livre.
   * **[Visualizações](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** - Visualizações, como um gráfico de barras ou de linhas, são adicionadas sobre os dados para dar vida visualmente a eles. No painel à esquerda, selecione o ícone do meio de Visualizações para ver a lista completa de visualizações disponíveis.
   * **[Painéis](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html)** - Um painel é uma coleção de tabelas e visualizações. Você pode acessar os painéis por meio do ícone superior esquerdo na Workspace. Os painéis são úteis quando você deseja organizar seus projetos de acordo com períodos, conjuntos de relatórios ou casos de uso de análise. Os seguintes tipos de painel estão disponíveis no Analysis Workspace:

   ![Seleção de Modelo](assets/create-analytics-workspace/workspace-tools.png)

### Adicionar visualização de dados com o Analysis Workspace

Em seguida, crie uma tabela para criar uma representação visual de como os usuários interagem com botões `Call to Action (CTA)` na página inicial do Site WKND. Para criar essa representação, vamos usar os dados coletados no [Componente de rastreamento clicado com o Adobe Analytics](./track-clicked-component.md). Abaixo está um resumo rápido dos dados rastreados para interações do usuário com os botões de Chamada para ação do site WKND.

* `eVar5` - `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Arraste e solte o componente de dimensão **Página** na Tabela de forma livre. Agora é possível exibir uma visualização que mostra o Nome da página (eVar9) e as Exibições de página correspondentes (Ocorrências) exibidas na tabela.

   ![Dimension da página](assets/create-analytics-workspace/evar9-dimension.png)

1. Arraste e solte a métrica **Clique no CTA** (evento8) na métrica de ocorrências e substitua-a. Agora você pode exibir uma visualização que exibe o Nome da página (eVar9) e uma contagem correspondente de eventos de cliques do CTA em uma página.

   ![Métrica De Página - Clique Em CTA](assets/create-analytics-workspace/evar8-cta-click.png)

1. Vamos analisar a página por seu tipo de modelo. Selecione a métrica de modelo de página dos componentes e arraste e solte a métrica Modelo de página na dimensão Nome da página. Agora é possível exibir o nome da página detalhado por seu tipo de modelo.

   * **Antes**
     ![eVar5](assets/create-analytics-workspace/evar5.png)

   * **Depois**
     ![Métricas do eVar5](assets/create-analytics-workspace/evar5-metrics.png)

1. Para entender como os usuários interagem com os botões do CTA quando estão nas páginas do site da WKND, é necessário detalhar ainda mais adicionando a métrica ID de botão (eVar8).

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Abaixo você pode ver uma representação visual do Site WKND detalhada por seu modelo de página e detalhada ainda mais por interação do usuário com os botões Clique para ação (CTA) do Site WKND.

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. Você pode substituir o valor ID do botão por um nome mais amigável usando as Classificações do Adobe Analytics. Você pode ler mais sobre como criar uma classificação para uma métrica específica [aqui](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html). Nesse caso, temos uma configuração da métrica de classificação `Button Section (Button ID)` para `eVar8` que mapeia a ID do botão para um nome amigável.

   ![Seção de Botões](assets/create-analytics-workspace/button-section.png)

## Adicionar classificação a uma variável analítica

### Classificações de conversão

A Classificação do Analytics é uma maneira de categorizar os dados de variáveis do Analytics e, em seguida, exibir os dados de diferentes maneiras ao gerar relatórios. Para melhorar como a ID do botão é exibida no relatório do Analytics Workspace, vamos criar uma variável de classificação para a ID do botão (eVar8). Ao classificar, você está estabelecendo uma relação entre a variável e os metadados relacionados a essa variável.

Em seguida, vamos criar uma Classificação para a variável do Analytics.

1. No menu da barra de ferramentas **Administrador**, selecione **Conjuntos de relatórios**
1. Selecione a **ID do Conjunto de Relatórios** na janela **Gerenciador de Conjunto de Relatórios** e clique em **Editar Configurações** > **Conversão** > **Classificações de Conversão**

   ![Classificação de conversão](assets/create-analytics-workspace/conversion-classification.png)

1. Na lista suspensa **Selecionar tipo de classificação**, selecione a variável (ID do botão eVar8) para adicionar uma classificação.
1. Clique na seta à direita ao lado da variável de classificação listada na seção Classificações para adicionar uma nova classificação.

   ![Tipo de Classificação de Conversão](assets/create-analytics-workspace/select-classification-variable.png)

1. Na caixa de diálogo **Editar uma Classificação**, forneça um nome adequado para a Classificação de Texto. Um componente de dimensão com o nome de Classificação de texto é criado.

   ![Tipo de Classificação de Conversão](assets/create-analytics-workspace/new-classification.png)

1. **Salve** suas alterações.

### Importador de classificação

Use o importador para carregar classificações no Adobe Analytics. Você também pode exportar os dados para atualização antes de uma importação. Os dados importados com a ferramenta de importação devem estar em um formato específico. O Adobe oferece a opção de baixar um modelo de dados com todos os detalhes do cabeçalho adequados em um arquivo de dados delimitado por tabulação. Você pode adicionar seus novos dados a esse modelo e, em seguida, importar o arquivo de dados no navegador usando FTP.

#### Modelo de classificação

Antes de importar classificações para relatórios de marketing, você pode baixar um modelo que ajuda a criar um arquivo de dados de classificações. O arquivo de dados usa suas classificações desejadas como cabeçalhos de coluna e, em seguida, organiza o conjunto de dados do relatório nos cabeçalhos de classificação apropriados.

Em seguida, vamos baixar o Modelo de classificação para a variável Button Id (eVar8)

1. Navegue até **Admin** > **Importador de classificação**
1. Vamos baixar um modelo de Classificação para a variável de conversão da guia **Baixar modelo**.
   ![Tipo de Classificação de Conversão](assets/create-analytics-workspace/classification-importer.png)

1. Na guia Download de modelo, especifique a configuração do modelo de dados.
   * **Selecionar Conjunto de Relatórios** : selecione o conjunto de relatórios a ser usado no modelo. O conjunto de relatórios e o conjunto de dados devem corresponder.
   * **Conjunto de Dados a ser classificado**: selecione o tipo de dados para o arquivo de dados. O menu inclui todos os relatórios em seus conjuntos de relatórios configurados para classificações.
   * **Codificação** : selecione a codificação de caracteres para o arquivo de dados. O formato de codificação padrão é UTF-8.

1. Clique em **Baixar** e salve o arquivo de modelo em seu sistema local. O arquivo de modelo é um arquivo de dados delimitado por tabulação (extensão de arquivo .tab) que a maioria dos aplicativos de planilha eletrônica suporta.
1. Abra o arquivo de dados delimitado por tabulação usando um editor de sua escolha.
1. Adicione a ID de botão (eVar9) e um nome de botão correspondente ao arquivo delimitado por tabulação para cada valor de eVar9 da Etapa 9 na seção.

   ![Valor da Chave](assets/create-analytics-workspace/key-value.png)

1. **Salve** o arquivo delimitado por tabulação.
1. Navegue até a guia **Importar Arquivo**.
1. Configure o Destino da importação de arquivo.
   * **Selecionar Conjunto de Relatórios** : WKND Site AEM (Conjunto de Relatórios)
   * **Conjunto de Dados a ser Classificado**: Id do Botão (Variável de Conversão eVar8)
1. Clique na opção **Escolher Arquivo** para carregar o arquivo delimitado por tabulação do sistema e clique em **Importar Arquivo**

   ![Importador de Arquivos](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Uma importação bem-sucedida exibe imediatamente as alterações apropriadas em uma exportação. No entanto, as alterações de dados em relatórios demoram até quatro horas ao usar uma importação de navegador e até 24 horas ao usar uma importação de FTP.

#### Substituir variável de conversão pela variável de classificação

1. Na barra de ferramentas do Analytics, selecione **Workspace** e abra o espaço de trabalho criado na seção [Criar um projeto no Analysis Workspace](#create-a-project-in-analysis-workspace) deste tutorial.

   ![ID do Botão Workspace](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Em seguida, substitua a métrica **Id do botão** no espaço de trabalho que exibe a ID de um botão de Chamada para ação (CTA) pelo nome de classificação criado na etapa anterior.

1. No localizador de componentes, pesquise por **Botões do WKND CTA** e arraste e solte a dimensão **Botões do WKND CTA (Id de Botão)** na métrica Id de Botão e substitua-a.

   * **Antes**
     ![Botão do Workspace Antes](assets/create-analytics-workspace/wknd-button-before.png)
   * **Depois**
     Botão ![Workspace Depois](assets/create-analytics-workspace/wknd-button-after.png)

1. É possível observar que a métrica da ID do botão que continha a ID do botão de um botão de Chamada para ação (CTA) agora é substituída por um nome correspondente fornecido no modelo de classificação.
1. Vamos comparar a tabela do Analytics Workspace com a página inicial da WKND e entender a contagem de cliques do botão do CTA e sua análise. Com base nos dados da tabela de forma livre do espaço de trabalho, está claro que 22 vezes os usuários clicaram no botão **SKI NOW** e quatro vezes para o Camping da Página Inicial do WKND na Austrália Ocidental **Ler mais**.

   ![Relatório do CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Salve seu projeto do Adobe Analytics Workspace e forneça um nome e uma descrição apropriados. Como opção, você pode adicionar tags a um projeto do Workspace.

   ![Salvar projeto](assets/create-analytics-workspace/save-project.png)

1. Depois de salvar o projeto com êxito, você pode compartilhar o projeto do espaço de trabalho com outros colegas de trabalho ou de equipe usando a opção Compartilhar.

   ![Compartilhar projeto](assets/create-analytics-workspace/share.png)

## Parabéns.

Você acabou de aprender a mapear dados capturados de um site do Adobe Experience Manager para métricas e dimensões em conjuntos de relatórios do Adobe Analytics. Além disso, o executou uma Classificação para as métricas e criou um painel de relatórios detalhado usando o recurso Analysis Workspace do Adobe Analytics.
