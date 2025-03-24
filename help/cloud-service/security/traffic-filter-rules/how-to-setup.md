---
title: Como configurar regras de Filtro de tráfego, incluindo regras do WAF
description: Saiba como configurar o para criar, implantar, testar e analisar os resultados das regras de Filtro de tráfego, incluindo regras do WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: b67bf642-3341-48d0-8ea9-5f262febf414
duration: 292
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 2%

---

# Como configurar regras de Filtro de tráfego, incluindo regras do WAF

Saiba **como configurar** regras de filtro de tráfego, incluindo regras do WAF. Leia sobre como criar, implantar, testar e analisar resultados.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## Configurar

O processo de configuração envolve o seguinte:

- _criando regras_ com uma estrutura de projeto e um arquivo de configuração apropriados do AEM.
- _implantando regras_ usando o pipeline de configuração do Adobe Cloud Manager.
- _testando regras_ usando várias ferramentas para gerar tráfego.
- _analisando os resultados_ usando logs de CDN e ferramentas de painel do AEMCS.

### Criar regras no seu projeto do AEM

Para criar regras, siga estas etapas:

1. No nível superior do seu projeto do AEM, crie uma pasta `config`.

1. Na pasta `config`, crie um novo arquivo chamado `cdn.yaml`.

1. Adicionar os seguintes metadados ao arquivo `cdn.yaml`:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

Veja um exemplo do arquivo `cdn.yaml` no Projeto do AEM Guides WKND Sites:

![Arquivo e pasta de regras de projeto do WKND AEM](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Implantar regras por meio do Cloud Manager {#deploy-rules-through-cloud-manager}

Para implantar as regras, siga estas etapas:

1. Faça logon no Cloud Manager em [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e selecione a organização e o programa apropriados.

1. Navegue até o cartão _Pipelines_ da página _Visão geral do programa_, clique no botão **+Adicionar** e selecione o tipo de pipeline desejado.

   ![Cartão Pipelines do Cloud Manager](./assets/cloud-manager-pipelines-card.png)

   No exemplo acima, para fins de demonstração, _Adicionar pipeline de não produção_ está selecionado, pois um ambiente dev é usado.

1. Na caixa de diálogo _Adicionar pipeline de não produção_, escolha e insira os seguintes detalhes:

   1. Etapa de configuração:

      - **Tipo**: pipeline de implantação
      - **Nome Do Pipeline**: Dev-Config

      ![Caixa de diálogo Pipeline de configuração do Cloud Manager](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Etapa do Source Code:

      - **Código para implantação**: implantação direcionada
      - **Incluir**: Configuração
      - **Ambiente de implantação**: nome do seu ambiente, por exemplo, wknd-program-dev.
      - **Repositório**: o repositório Git de onde o pipeline deve recuperar o código; por exemplo, `wknd-site`
      - **Ramificação Git**: o nome da ramificação do repositório Git.
      - **Localização do Código**: `/config`, correspondente à pasta de configuração de nível superior criada na etapa anterior.

      ![Caixa de diálogo Pipeline de configuração do Cloud Manager](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Testar regras gerando tráfego

Para testar as regras, há várias ferramentas de terceiros disponíveis e sua organização pode ter uma ferramenta preferencial. Para o propósito da demonstração, vamos usar as seguintes ferramentas:

- [Curl](https://curl.se/) para testes básicos, como invocar uma URL e verificar o código de resposta.

- [Vegeta](https://github.com/tsenart/vegeta) para executar negação de serviço (DOS). Siga as instruções de instalação do [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) para encontrar possíveis problemas e vulnerabilidades de segurança, como XSS, injeção de SQL e muito mais. Siga as instruções de instalação do [Nikto GitHub](https://github.com/sullo/nikto).

- Verifique se as ferramentas estão instaladas e disponíveis no terminal executando os comandos abaixo:

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Analisar resultados usando a ferramenta do painel

Depois de criar, implantar e testar as regras, você pode analisar os resultados usando os logs do **CDN** e o **AEMCS-CDN-Log-Analysis-Tooling**. A ferramenta fornece um conjunto de painéis para visualizar os resultados para a pilha Splunk e ELK (Elasticsearch, Logstash e Kibana).

A ferramenta pode ser clonada do [repositório GitHub AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Em seguida, siga as instruções para instalar e carregar os painéis do **Painel de Tráfego do CDN** e do **Painel do WAF** para sua ferramenta de observabilidade preferida.

Neste tutorial, vamos usar a pilha ELK. Siga as instruções do [contêiner ELK Docker da Análise de Log da CDN do AEM CS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) para configurar a pilha ELK.

- Depois de carregar o painel de amostra, a página de ferramenta do painel Elástico deve ser semelhante ao seguinte:

  ![Painel de Regras de Filtro de Tráfego ELK](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Como ainda não há logs CDN do AEMCS assimilados, o painel fica vazio.


## Próxima etapa

Saiba como declarar regras de filtro de tráfego, incluindo regras do WAF no capítulo [Exemplos e análise de resultados](./examples-and-analysis.md), usando o Projeto de sites WKND do AEM.
