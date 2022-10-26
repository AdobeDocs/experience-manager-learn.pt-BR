---
title: Analisar dados com o Analysis Workspace
description: Saiba como mapear dados capturados de um site do Adobe Experience Manager para métricas e dimensões em conjuntos de relatórios do Adobe Analytics. Saiba como criar um painel de relatórios detalhado usando o recurso Analysis Workspace do Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: User
level: Intermediate
kt: 6409
thumbnail: KT-6296.jpg
exl-id: b5722fe2-93bf-4b25-8e08-4cb8206771cb
last-substantial-update: 2022-06-15T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '2177'
ht-degree: 0%

---

# Analisar dados com o Analysis Workspace

Saiba como mapear dados capturados de um site do Adobe Experience Manager para métricas e dimensões em conjuntos de relatórios do Adobe Analytics. Saiba como criar um painel de relatórios detalhado usando o recurso Analysis Workspace do Adobe Analytics.

## O que você vai criar

A equipe de marketing da WKND quer entender quais botões de Ação de Chamada (CTA) têm melhor desempenho na página inicial. Neste tutorial, criaremos um novo projeto no Analysis Workspace para visualizar o desempenho de diferentes botões CTA e entender o comportamento do usuário no site. As seguintes informações são capturadas usando o Adobe Analytics quando um usuário clica em um botão de Ação de Chamada (CTA) na página inicial da WKND.

**Variáveis do Analytics**

Abaixo estão as variáveis do Analytics que estão sendo rastreadas no momento:

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

![CTA Clique em Adobe Analytics](assets/create-analytics-workspace/page-analytics.png)

### Objetivos {#objective}

1. Crie um novo Conjunto de relatórios ou use um existente.
1. Configurar [Variáveis de conversão (eVars)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html) e [Eventos bem-sucedidos (Eventos)](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/success-events/success-event.html) no Conjunto de relatórios.
1. Crie um [Projeto Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html) para analisar dados com a ajuda de ferramentas que permitem criar, analisar e compartilhar insights rapidamente.
1. Compartilhe o projeto do Analysis Workspace com outros membros da equipe.

## Pré-requisitos

Este tutorial é uma continuação do [Rastrear componente clicado com o Adobe Analytics](./track-clicked-component.md) e parte do princípio que você tem:

* A **Propriedade do Launch** com o [Extensão do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) ativado
* **Adobe Analytics** ID do conjunto de relatórios de teste/desenvolvimento e servidor de rastreamento. Consulte a documentação a seguir para [criação de um novo conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) extensão do navegador configurada com a propriedade do Launch carregada em [https://wknd.site/us/en.html](https://wknd.site/us/en.html) ou um site AEM com a Camada de dados Adobe.

## Variáveis de conversão (eVars) e Eventos bem-sucedidos (Evento)

A variável de conversão do Custom Insight (ou eVar) é colocada no código Adobe nas páginas da Web selecionadas do site. Seu objetivo principal é segmentar métricas de sucesso de conversão em relatórios de marketing personalizados. Um eVar pode ser baseado em visitas e funcionar de forma semelhante aos cookies. Os valores passados para variáveis de eVar seguem o usuário por um período predeterminado.

Quando um eVar é definido como o valor de um visitante, o Adobe lembra automaticamente esse valor até sua expiração. Quaisquer eventos bem-sucedidos que um visitante encontra enquanto o valor do eVar está ativo são contados em relação ao valor do eVar.

As eVars são mais bem usadas para medir causa e efeito, como:

* Quais campanhas internas influenciaram a receita
* Quais anúncios em banner resultaram em um registro
* O número de vezes que uma pesquisa interna foi usada antes de fazer um pedido

Eventos bem-sucedidos são ações que podem ser rastreadas. Você determina o que é um evento bem-sucedido. Por exemplo, se um visitante clica em um botão CTA, o evento click pode ser considerado um evento bem-sucedido.

### Configurar eVars

1. Na página inicial da Adobe Experience Cloud, selecione sua organização e inicie o Adobe Analytics.

   ![Analytics AEP](assets/create-analytics-workspace/analytics-aep.png)

1. Na barra de ferramentas do Analytics, clique em **Administrador** > **Conjuntos de relatórios** e encontre seu Conjunto de relatórios.

   ![Conjunto de relatórios do Analytics](assets/create-analytics-workspace/select-report-suite.png)

1. Selecione o Conjunto de relatórios > **Editar configurações** > **Conversão** > **Variáveis de conversão**

   ![Variáveis de conversão do Analytics](assets/create-analytics-workspace/conversion-variables.png)

1. Usar o **Adicionar novo** , criaremos Variáveis de conversão para mapear o esquema como abaixo:

   * `eVar5` -  `Page Template`
   * `eVar6` - `Page ID`
   * `eVar7` - `Last Modified Date`
   * `eVar8` - `Button Id`
   * `eVar9` - `Page Name`

   ![Adicionar novas eVars](assets/create-analytics-workspace/add-new-evars.png)

1. Forneça um nome e uma descrição apropriados para cada eVars e **Salvar** suas alterações. Usamos essas eVars para criar um projeto do Analysis Workspace na próxima seção. Assim, um nome amigável torna as variáveis facilmente descobertas.

   ![eVars](assets/create-analytics-workspace/evars.png)

### Configurar eventos bem-sucedidos

Em seguida, vamos criar um evento para rastrear o clique do botão CTA.

1. No **Gerenciador do Conjunto de relatórios** selecione a **Id Do Conjunto De Relatórios** e clique em **Editar configurações**.
1. Clique em **Conversão** > **Eventos bem-sucedidos**
1. Usar o **Adicionar novo** , crie um novo evento bem-sucedido personalizado para rastrear o clique do botão CTA e **Salvar** suas alterações.
   * `Event` : `event8`
   * `Name`:`CTA Click`
   * `Type`:`Counter`

   ![eVars](assets/create-analytics-workspace/add-success-event.png)

## Criar um novo projeto no Analysis Workspace {#workspace-project}

O Analysis Workspace é uma ferramenta de navegador flexível que permite criar análises e compartilhar insights rapidamente. Usando a interface de arrastar e soltar, você pode criar sua análise, adicionar visualizações para dar vida aos dados, preparar um conjunto de dados, compartilhar e agendar projetos com qualquer pessoa em sua organização.

Em seguida, crie um novo [projeto](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/freeform-overview.html#analysis-workspace) para criar um painel para analisar o desempenho dos botões CTA em todo o site.

1. Na barra de ferramentas do Analytics, selecione **Workspace** e clique em para **Criar um novo projeto**.

   ![Espaço de trabalho](assets/create-analytics-workspace/create-workspace.png)

1. Escolha iniciar a partir de um **projeto em branco** ou selecione um dos modelos pré-criados, fornecido pelo Adobe ou modelos personalizados criados por sua organização. Vários modelos estão disponíveis, dependendo da análise ou caso de uso que você tem em mente. [Saiba mais](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/build-workspace-project/starter-projects.html) sobre as diferentes opções de modelo disponíveis.

   No projeto do Workspace, painéis, tabelas, visualizações e componentes são acessados no painel esquerdo. Estes são os componentes do projeto.

   * **[Componentes](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/components/analysis-workspace-components.html)** - Componentes são dimensões, métricas, segmentos ou intervalos de datas que podem ser combinados em uma tabela de forma livre para começar a responder suas perguntas comerciais. Familiarize-se com cada tipo de componente antes de mergulhar na análise. Depois de dominar a terminologia do componente, você pode começar a arrastar e soltar para criar a análise em uma tabela de Forma livre.
   * **[Visualizações](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/visualizations/freeform-analysis-visualizations.html)** - Visualizações, como um gráfico de barras ou de linhas, são adicionadas sobre os dados para dar vida visualmente a eles. No painel à esquerda, selecione o ícone do meio de Visualizações para ver a lista completa de visualizações disponíveis.
   * **[Painéis](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/panels/panels.html)** - Um painel é uma coleção de tabelas e visualizações. Você pode acessar painéis usando o ícone superior esquerdo no Workspace. Painéis são úteis quando você deseja organizar seus projetos de acordo com períodos de tempo, conjuntos de relatórios ou casos de uso de análise. Os seguintes tipos de painéis estão disponíveis no Analysis Workspace:

   ![Seleção de modelo](assets/create-analytics-workspace/workspace-tools.png)

### Adicionar visualização de dados com o Analysis Workspace

Em seguida, crie uma tabela para criar uma representação visual de como os usuários interagem com os botões de Ação de Chamada (CTA) na página inicial do Site WKND. Para criar essa representação, vamos usar os dados coletados no [Rastrear componente clicado com o Adobe Analytics](./track-clicked-component.md). Abaixo está um rápido resumo dos dados rastreados para interações do usuário com os botões de Ação de Chamada para o Site WKND.

* `eVar5` -  `Page template`
* `eVar6` - `Page Id`
* `eVar7` - `Page last modified date`
* `eVar8` - `CTA Button Id`
* `eVar9` - `Page Name`
* `event8` - `CTA Button Click event`
* `prop8` - `CTA Button Id`

1. Arraste e solte a **Página** componente de dimensão na tabela de forma livre. Agora é possível visualizar uma visualização que exibe o Nome da página (eVar9) e as Exibições de página correspondentes (Ocorrências) exibidas dentro da tabela.

   ![Dimension da página](assets/create-analytics-workspace/evar9-dimension.png)

1. Arraste e solte a **Clique CTA** (event8) na métrica ocorrências e substitua-a. Agora é possível visualizar uma visualização que exibe o Nome da página (eVar 9) e uma contagem correspondente de eventos de CTA Click em uma página.

   ![Métrica da página - Clique no CTA](assets/create-analytics-workspace/evar8-cta-click.png)

1. Vamos analisar a página por tipo de modelo. Selecione a métrica de modelo de página dos componentes e arraste e solte a métrica Modelo de página na dimensão Nome da página . Agora é possível visualizar o nome da página detalhado pelo tipo de modelo.

   * **Antes**

      ![eVar 5](assets/create-analytics-workspace/evar5.png)

   * **Depois**

      ![Métricas do eVar5](assets/create-analytics-workspace/evar5-metrics.png)

1. Para entender como os usuários interagem com botões CTA quando estão nas páginas do site WKND, precisamos detalhar ainda mais a métrica Modelo de página adicionando a métrica ID do botão (eVar8).

   ![eVar8](assets/create-analytics-workspace/evar8.png)

1. Abaixo, você pode ver uma representação visual do Site WKND dividida por seu modelo de página e ainda mais detalhada por interação do usuário com os botões de ação de clique do site WKND (CTA).

   ![eVar8](assets/create-analytics-workspace/evar8-metric.png)

1. É possível substituir o valor da ID do botão por um nome mais simples, usando as Classificações do Adobe Analytics. Você pode ler mais sobre como criar uma classificação para uma métrica específica [here](https://experienceleague.adobe.com/docs/analytics/components/classifications/c-classifications.html). Nesse caso, temos uma métrica de classificação `Button Section (Button ID)` configurar para `eVar8` que mapeia a id do botão para um nome amigável.

   ![Seção do botão](assets/create-analytics-workspace/button-section.png)

## Adicionar classificação a uma variável do Analytics

### Classificações de conversão

A Classificação do Analytics é uma maneira de categorizar os dados variáveis do Analytics, em seguida, exibir os dados de maneiras diferentes ao gerar os relatórios. Para melhorar como a ID do botão é exibida no relatório do Analysis Workspace, vamos criar uma variável de classificação para a ID do botão (eVar8). Ao classificar, você está estabelecendo uma relação entre a variável e os metadados relacionados a ela.

Em seguida, vamos criar uma Classificação para a variável do Analytics .

1. No **Administrador** menu da barra de ferramentas, selecione **Conjuntos de relatórios**
1. Selecione o **Id Do Conjunto De Relatórios** do **Gerenciador do Conjunto de relatórios** e clique em **Editar configurações** > **Conversão** > **Classificações de conversão**

   ![Classificação de conversão](assets/create-analytics-workspace/conversion-classification.png)

1. No **Selecionar tipo de classificação** na lista suspensa, selecione a variável (eVar8-Button ID) para adicionar uma classificação.
1. Clique na seta ao lado da variável Classificação listada na seção Classificações para adicionar uma nova Classificação.

   ![Tipo de classificação de conversão](assets/create-analytics-workspace/select-classification-variable.png)

1. No **Editar uma classificação** , forneça um nome adequado para a Classificação de texto. Um componente de dimensão com o nome da Classificação de texto é criado.

   ![Tipo de classificação de conversão](assets/create-analytics-workspace/new-classification.png)

1. **Salvar** suas alterações.

### Classificação do importador

Use o importador para fazer upload de classificações no Adobe Analytics. Também é possível exportar os dados para atualização antes de uma importação. Os dados importados por meio da ferramenta de importação devem estar em um formato específico. O Adobe fornece a opção para baixar um modelo de dados com todos os detalhes de cabeçalho adequados em um arquivo de dados delimitado por tabulação. Você pode adicionar seus novos dados a esse modelo e, em seguida, importar o arquivo de dados no navegador usando FTP.

#### Modelo de classificação

Antes de importar as classificações para os relatórios de marketing, é possível baixar um modelo que ajuda a criar um arquivo de dados de classificações. O arquivo de dados usa suas classificações desejadas como cabeçalhos de coluna e, em seguida, organiza o conjunto de dados de relatório nos cabeçalhos de classificação apropriados.

Em seguida, vamos baixar o Modelo de classificação para a variável ID do botão (eVar8)

1. Navegar para **Administrador** > **Classificação do importador**
1. Vamos baixar um modelo de classificação para a variável de conversão da variável **Fazer download do modelo** Guia.
   ![Tipo de classificação de conversão](assets/create-analytics-workspace/classification-importer.png)

1. Na guia Download de modelo , especifique a configuração do modelo de dados.
   * **Selecionar Conjunto de relatórios** : Selecione o conjunto de relatórios a ser usado no modelo. O conjunto de relatórios e o conjunto de dados devem corresponder.
   * **Conjunto de dados a ser classificado** : Selecione o tipo de dados para o arquivo de dados. O menu inclui todos os relatórios nos conjuntos de relatórios configurados para classificações.
   * **Codificação** : Selecione a codificação de caracteres para o arquivo de dados. O formato de codificação padrão é UTF-8.

1. Clique em **Baixar** e salve o arquivo de modelo em seu sistema local. O arquivo de modelo é um arquivo de dados delimitado por tabulação (extensão de arquivo .tab ) que a maioria dos aplicativos de planilha eletrônica suporta.
1. Abra o arquivo de dados delimitado por tabulação usando um editor de sua escolha.
1. Adicione a ID do botão (eVar9) e um nome de botão correspondente ao arquivo delimitado por tabulação para cada valor de eVar9 da Etapa 9 da seção .

   ![Valor-chave](assets/create-analytics-workspace/key-value.png)

1. **Salvar** o arquivo delimitado por tabulação.
1. Navegue até o **Importar arquivo** guia .
1. Configure o Destino para a importação de arquivo.
   * **Selecionar Conjunto de relatórios** : AEM de site WKND (conjunto de relatórios)
   * **Conjunto de dados a serem classificados** : Id Do Botão (eVar Da Variável De Conversão 8)
1. Clique no botão **Escolher arquivo** para fazer upload do arquivo delimitado por tabulação do sistema e, em seguida, clique em **Importar arquivo**

   ![Importador de arquivos](assets/create-analytics-workspace/file-importer.png)

   >[!NOTE]
   >
   > Uma importação bem-sucedida exibe imediatamente as alterações apropriadas em uma exportação. No entanto, as alterações de dados nos relatórios levam até quatro horas quando se usa uma importação de navegador e até 24 horas quando se usa uma importação FTP.

#### Substitua a variável de conversão pela variável de classificação

1. Na barra de ferramentas do Analytics, selecione **Workspace** e abra o espaço de trabalho criado em [Criar um novo projeto no Analysis Workspace](#workspace-project) deste tutorial.

   ![ID do botão do espaço de trabalho](assets/create-analytics-workspace/workspace-report-button-id.png)

1. Em seguida, substitua o **Id Do Botão** na área de trabalho que exibe a ID de um botão de Ação de chamada (CTA) com o nome de classificação criado na etapa anterior.

1. No localizador de componentes, procure por **Botões CTA WKND** e arraste e solte a **Botões CTA WKND (Id Do Botão)** na métrica ID do botão e substitua-a.

   * **Antes**

      ![Botão Espaço de Trabalho Antes](assets/create-analytics-workspace/wknd-button-before.png)
   * **Depois**

      ![Botão Espaço de trabalho depois de](assets/create-analytics-workspace/wknd-button-after.png)

1. Você pode observar que a métrica de ID do botão que continha a ID do botão de um botão de Ação (CTA) agora é substituída por um nome correspondente fornecido no Modelo de classificação.
1. Vamos comparar a tabela do Analytics Workspace com a página inicial da WKND e entender a contagem de cliques do botão CTA e sua análise. Com base nos dados da tabela de forma livre do espaço de trabalho, é claro que 22 vezes os usuários clicaram no **SKI AGORA** botão e quatro vezes para a página inicial WKND Camping in Western Austrália **Leia mais** botão.

   ![Relatório CTA](assets/create-analytics-workspace/workspace-report-buttons-wknd.png)

1. Salve o projeto do Adobe Analytics Workspace e forneça um nome e uma descrição adequados. Como opção, você pode adicionar tags a um projeto do espaço de trabalho.

   ![Salvar projeto](assets/create-analytics-workspace/save-project.png)

1. Depois de salvar seu projeto com êxito, você pode compartilhar seu projeto do espaço de trabalho com outros colegas de trabalho ou colegas de equipe usando a opção Compartilhar .

   ![Compartilhar projeto](assets/create-analytics-workspace/share.png)

## Parabéns. 

Você acabou de aprender a mapear dados capturados de um site do Adobe Experience Manager para métricas e dimensões em conjuntos de relatórios do Adobe Analytics, executar uma Classificação para as métricas e criar um painel de relatórios detalhado usando o recurso Analysis Workspace do Adobe Analytics.
