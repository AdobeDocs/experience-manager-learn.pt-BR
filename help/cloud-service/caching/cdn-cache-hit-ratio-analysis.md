---
title: Análise da taxa de acertos do cache do CDN
description: Saiba como analisar os logs de CDN fornecidos pela AEM as a Cloud Service. Obtenha insights, como a taxa de acertos do cache e os principais URLs dos tipos de cache MISS e PASS para fins de otimização.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-11-10T00:00:00Z
jira: KT-13312
thumbnail: KT-13312.jpeg
exl-id: 43aa7133-7f4a-445a-9220-1d78bb913942
duration: 276
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1476'
ht-degree: 0%

---

# Análise da taxa de acertos do cache do CDN

O conteúdo armazenado em cache na CDN reduz a latência experimentada pelos usuários do site, que não precisam aguardar a solicitação para retornar ao Apache/Dispatcher ou à publicação do AEM. Com isso em mente, vale a pena otimizar a taxa de ocorrência do cache do CDN para maximizar a quantidade de conteúdo armazenável em cache no CDN.

Saiba como analisar os **logs de CDN** fornecidos pela AEM as a Cloud Service e obter insights, como **taxa de acertos de cache** e **principais URLs de _MISS_ e _PASS_ tipos de cache**, para fins de otimização.


Os logs CDN estão disponíveis no formato JSON, que contém vários campos, incluindo `url`, `cache`. Para obter mais informações, consulte o [Formato de Log da CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/logging.html?lang=pt-BR#cdn-log:~:text=Toggle%20Text%20Wrapping-,Log%20Format,-The%20CDN%20logs). O campo `cache` fornece informações sobre o _estado do cache_ e seus valores possíveis são HIT, MISS ou PASS. Vamos analisar os detalhes de valores possíveis.

| Estado de Cache </br> Valor Possível | Descrição |
|------------------------------------|:-----------------------------------------------------:|
| HIT | Os dados solicitados são _encontrados no cache da CDN e não requerem uma solicitação de busca_ para o servidor do AEM. |
| SENHORITA | Os dados solicitados são _não encontrados no cache CDN e devem ser solicitados_ do servidor AEM. |
| PASS | Os dados solicitados são _explicitamente definidos para não serem armazenados em cache_ e sempre serem recuperados do servidor do AEM. |

Para fins deste tutorial, o [projeto WKND do AEM](https://github.com/adobe/aem-guides-wknd) é implantado no ambiente do AEM as a Cloud Service e um pequeno teste de desempenho é acionado usando o [Apache JMeter](https://jmeter.apache.org/).

Este tutorial está estruturado para orientá-lo pelo seguinte processo:

1. Download de logs CDN por meio do Cloud Manager
1. A análise desses logs de CDN pode ser executada com duas abordagens: um painel instalado localmente ou um Splunk ou Jupityer Notebook acessado remotamente (para aqueles que licenciam o Adobe Experience Platform)
1. Otimização da configuração do cache da CDN

## Baixar logs CDN

Para baixar os logs de CDN, siga estas etapas:

1. Faça logon no Cloud Manager em [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e selecione sua organização e programa.

1. Para um ambiente AEMCS desejado, selecione **Baixar logs** no menu de reticências.

   ![Baixar Logs - Cloud Manager](assets/cdn-logs-analysis/download-logs.png){width="500" zoomable="yes"}

1. Na caixa de diálogo **Baixar Logs**, selecione o Serviço **Publicar** no menu suspenso e clique no ícone de download ao lado da linha **CDN**.

   ![Logs da CDN - Cloud Manager](assets/cdn-logs-analysis/download-cdn-logs.png){width="500" zoomable="yes"}


Se o arquivo de log baixado for de _hoje_, a extensão de arquivo será `.log`; caso contrário, para arquivos de log anteriores, a extensão será `.log.gz`.

## Analisar logs de CDN baixados

Para obter insights, como a taxa de acertos do cache e os principais URLs dos tipos de cache MISS e PASS, analise o arquivo de log de CDN baixado. Esses insights ajudam a otimizar a [configuração de cache da CDN](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching) e aprimorar o desempenho do site.

Para analisar os logs de CDN, este tutorial apresenta três opções:

1. **Elasticsearch, Logstash e Kibana (ELK)**: a [ferramenta do painel ELK](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) pode ser instalada localmente.
1. **Splunk**: a [ferramenta do painel Splunk](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) requer acesso ao Splunk e ao [encaminhamento de logs do AEMCS habilitado](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) para assimilar os logs de CDN.
1. **Jupyter Notebook**: ele pode ser acessado remotamente como parte do [Adobe Experience Platform](https://experienceleague.adobe.com/pt-br/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data) sem instalar software adicional para clientes que possuem Adobe Experience Platform licenciado.

### Opção 1: usar ferramentas de painel ELK

A [pilha de ELK](https://www.elastic.co/elastic-stack) é um conjunto de ferramentas que fornecem uma solução escalável para pesquisar, analisar e visualizar os dados. Consiste em Elasticsearch, Logstash e Kibana.

Para identificar os detalhes principais, vamos usar o projeto [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Este projeto fornece um contêiner Docker da pilha ELK e um painel Kibana pré-configurado para analisar os logs CDN.

1. Siga as etapas de [Como configurar o contêiner ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) e certifique-se de importar o painel Kibana da **Taxa de Acertos do Cache do CDN**.

1. Para identificar a taxa de acertos do cache do CDN e os URLs principais, siga estas etapas:

   1. Copie o(s) arquivo(s) de log de CDN baixado(s) dentro da pasta de logs específicos do ambiente, por exemplo, `ELK/logs/stage`.

   1. Abra o painel **Taxa de Acertos do Cache do CDN** clicando no canto superior esquerdo _Menu de Navegação > Analytics > Painel > Taxa de Acertos do Cache do CDN_.

      ![Taxa de acertos do cache do CDN - Painel Kibana](assets/cdn-logs-analysis/cdn-cache-hit-ratio-dashboard.png){width="500" zoomable="yes"}

   1. Selecione o intervalo de tempo desejado no canto superior direito.

      ![Intervalo de tempo - Painel Kibana](assets/cdn-logs-analysis/time-range.png){width="500" zoomable="yes"}

   1. O painel **Taxa de Acertos do Cache do CDN** é autoexplicativo.

   1. A seção _Análise de Solicitação Total_ exibe os seguintes detalhes:
      - Taxas de cache por tipo de cache
      - Contagens de cache por tipo de cache

      ![Análise de Solicitação Total - Painel Kibana](assets/cdn-logs-analysis/total-request-analysis.png){width="500" zoomable="yes"}

   1. A _Análise por Solicitação ou Tipos MIME_ exibe os seguintes detalhes:
      - Taxas de cache por tipo de cache
      - Contagens de cache por tipo de cache
      - Principais URLs MISS e PASS

      ![Análise por Solicitação ou Tipos Mime - Painel Kibana](assets/cdn-logs-analysis/analysis-by-request-or-mime-types.png){width="500" zoomable="yes"}

#### Filtrar por nome de ambiente ou ID de programa

Para filtrar os logs assimilados por nome de ambiente, siga as etapas abaixo:

1. No painel Taxa de Acertos do Cache CDN, clique no ícone **Adicionar Filtro**.

   ![Filtro - Painel Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. Na modal **Adicionar filtro**, selecione o campo `aem_env_name.keyword` no menu suspenso e o operador `is` e o nome de ambiente desejado para o próximo campo e, por fim, clique em _Adicionar filtro_.

   ![Adicionar filtro - Painel Kibana](assets/cdn-logs-analysis/add-filter.png){width="500" zoomable="yes"}

#### Filtrar por nome de host

Para filtrar os logs assimilados por nome de host, siga as etapas abaixo:

1. No painel Taxa de Acertos do Cache CDN, clique no ícone **Adicionar Filtro**.

   ![Filtro - Painel Kibana](assets/cdn-logs-analysis/filter.png){width="500" zoomable="yes"}

1. No modal **Adicionar filtro**, selecione o campo `host.keyword` no menu suspenso e o operador `is` e o nome de host desejado para o próximo campo e, por fim, clique em _Adicionar filtro_.

   ![Filtro de Host - Painel do Kibana](assets/cdn-logs-analysis/add-host-filter.png){width="500" zoomable="yes"}

Da mesma forma, adicione mais filtros ao painel com base nos requisitos de análise.

### Opção 2: usar a ferramenta Painel do Splunk

O [Splunk](https://www.splunk.com/) é uma ferramenta de análise de log popular que ajuda a agregar, analisar logs e criar visualizações para fins de monitoramento e solução de problemas.

Para identificar os detalhes principais, vamos usar o projeto [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Este projeto fornece um painel do Splunk para analisar os logs do CDN.

1. Siga as etapas dos [painéis do Splunk para a Análise de Log da CDN do AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md) e importe o painel **Taxa de Acertos do Cache do CDN**.
1. Se necessário, atualize o Índice _, o Tipo de Source e outros valores de filtro_ no painel do Splunk.

   ![Painel do Splunk](assets/cdn-logs-analysis/splunk-CHR-dashboard.png){width="500" zoomable="yes"}

>[!NOTE]
>
>A interface e os gráficos no painel do splunk diferem do painel ELK, no entanto, os detalhes principais são semelhantes.

### Opção 3: usar o Jupyter Notebook

Para aqueles que preferem não instalar o software localmente (ou seja, a ferramenta do painel ELK da seção anterior), há outra opção, mas ela requer uma licença para a Adobe Experience Platform.

O [Jupyter Notebook](https://jupyter.org/) é um aplicativo web de código aberto que permite criar documentos que contêm código, texto e visualização. Ele é usado para transformação de dados, visualização e modelagem estatística. Ele pode ser acessado remotamente [como parte do Adobe Experience Platform](https://experienceleague.adobe.com/pt-br/docs/experience-platform/data-science-workspace/jupyterlab/analyze-your-data).

#### Baixando o arquivo de Bloco de Anotações Python Interativo

Primeiro, baixe o arquivo [AEM-as-a-CloudService - CDN Logs Analysis - Jupyter Notebook](./assets/cdn-logs-analysis/aemcs_cdn_logs_analysis.ipynb), que ajudará na análise de logs CDN. Esse arquivo &quot;Interative Python Notebook&quot; é autoexplicativo, no entanto, os principais destaques de cada seção são:

- **Instalar bibliotecas adicionais**: instala as bibliotecas Python `termcolor` e `tabulate`.
- **Carregar logs CDN**: carrega o arquivo de log CDN usando o valor de variável `log_file`. Atualize seu valor. Ele também transforma este log de CDN no [DataFrame Pandas](https://pandas.pydata.org/docs/reference/frame.html).
- **Executar análise**: o primeiro bloco de código é _Exibir Resultado da Análise para Total, HTML, JS/CSS e Solicitações de Imagem_; ele fornece gráficos de porcentagem de taxa de acertos de cache, de barras e de pizza.
O segundo bloco de código é _Top 5 URLs de solicitação de MISS e PASS para HTML, JS/CSS e Image_; ele exibe URLs e suas contagens em formato de tabela.

#### Execução do Jupyter Notebook

Em seguida, execute o Jupyter Notebook no Adobe Experience Platform, seguindo estas etapas:

1. Faça logon no [Adobe Experience Cloud](https://experience.adobe.com/), na página inicial > seção **Acesso rápido** > clique no **Experience Platform**

   ![Experience Platform](assets/cdn-logs-analysis/experience-platform.png){width="500" zoomable="yes"}

1. Na página inicial do Adobe Experience Platform > seção Data Science >, clique no item de menu **Blocos de anotações**. Para iniciar o ambiente do Jupyter Notebooks, clique na guia **JupyterLab**.

   ![Atualização de Valor do Arquivo de Log do Bloco de Anotações](assets/cdn-logs-analysis/datascience-notebook.png){width="500" zoomable="yes"}

1. No menu do JupyterLab, usando o ícone **Carregar Arquivos**, carregue o arquivo de log CDN baixado e o arquivo `aemcs_cdn_logs_analysis.ipynb`.

   ![Carregar arquivos - JupyteLab](assets/cdn-logs-analysis/jupyterlab-upload-file.png){width="500" zoomable="yes"}

1. Abra o arquivo `aemcs_cdn_logs_analysis.ipynb` clicando duas vezes.

1. Na seção **Carregar Arquivo de Log da CDN** do bloco de anotações, atualize o valor `log_file`.

   ![Atualização de Valor do Arquivo de Log do Bloco de Anotações](assets/cdn-logs-analysis/notebook-update-variable.png){width="500" zoomable="yes"}

1. Para executar a célula selecionada e avançar, clique no ícone **Reproduzir**.

   ![Atualização de Valor do Arquivo de Log do Bloco de Anotações](assets/cdn-logs-analysis/notebook-run-cell.png){width="500" zoomable="yes"}

1. Após executar a célula de código **Exibir Resultado da Análise para Total, HTML, JS/CSS e Solicitações de Imagem**, a saída exibe os gráficos de porcentagem da taxa de acertos do cache, barra e pizza.

   ![Atualização de Valor do Arquivo de Log do Bloco de Anotações](assets/cdn-logs-analysis/output-cache-hit-ratio.png){width="500" zoomable="yes"}

1. Depois de executar as **Principais 5 URLs de solicitação de MISS e PASS para HTML, JS/CSS e célula de código de Imagem**, a saída exibe as 5 Principais URLs de solicitação de MISS e PASS.

   ![Atualização de Valor do Arquivo de Log do Bloco de Anotações](assets/cdn-logs-analysis/output-top-urls.png){width="500" zoomable="yes"}

Você pode aprimorar o Jupyter Notebook para analisar os logs de CDN com base em seus requisitos.

## Otimização da configuração do cache da CDN

Depois de analisar os logs de CDN, você pode otimizar a configuração do cache de CDN para melhorar o desempenho do site. A prática recomendada do AEM é ter uma taxa de acertos de cache de 90% ou superior.

Para obter mais informações, consulte [Otimizar a configuração do cache da CDN](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching).

O projeto WKND do AEM tem uma configuração de CDN de referência. Para obter mais informações, consulte [Configuração de CDN](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L137-L190) do arquivo `wknd.vhost`.
