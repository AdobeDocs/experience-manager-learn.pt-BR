---
title: Como configurar regras de Filtro de tráfego incluindo regras WAF
description: Saiba como configurar para criar, implantar, testar e analisar os resultados das regras de Filtro de tráfego, incluindo regras WAF.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 2%

---

# Como configurar regras de Filtro de tráfego incluindo regras WAF

Saiba mais **como configurar** regras de filtro de tráfego, incluindo regras WAF. Leia sobre como criar, implantar, testar e analisar resultados.

>[!VIDEO](https://video.tv.adobe.com/v/3425407?quality=12&learn=on)

## Configurar

O processo de configuração envolve o seguinte:

- _criação de regras_ com uma estrutura de projeto AEM apropriada e um arquivo de configuração.
- _implantação de regras_ usando o pipeline de configuração do Adobe Cloud Manager.
- _regras de teste_ usar várias ferramentas para gerar tráfego.
- _análise dos resultados_ usar logs de CDN e ferramentas de painel do AEM CS.

### Criar regras no projeto AEM

Para criar regras, siga estas etapas:

1. No nível superior do projeto AEM, crie uma pasta `config`.

1. No prazo de `config` , crie um novo arquivo chamado `cdn.yaml`.

1. Adicione os seguintes metadados à `cdn.yaml` arquivo:

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

Veja um exemplo de `cdn.yaml` arquivo no projeto de sites WKND de guias do AEM:

![Arquivo e pasta de regras do projeto AEM WKND](./assets/wknd-rules-file-and-folder.png){width="800" zoomable="yes"}

### Implantar regras por meio do Cloud Manager {#deploy-rules-through-cloud-manager}

Para implantar regras, siga estas etapas:

1. Faça logon no Cloud Manager em [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e selecione a organização e o programa apropriado.

1. Navegue até a _Pipelines_ do _Visão geral do programa_ e clique no link **+Adicionar** e selecione o tipo de pipeline desejado.

   ![Cartão Pipelines do Cloud Manager](./assets/cloud-manager-pipelines-card.png)

   No exemplo acima, para fins de demonstração _Adicionar pipeline de não produção_ está selecionado desde que um ambiente dev é usado.

1. No _Adicionar pipeline de não produção_ escolha e insira os seguintes detalhes:

   1. Etapa de configuração:

      - **Tipo**: Pipeline de implantação
      - **Nome do pipeline**: Dev-Config

      ![Caixa de diálogo de configuração do pipeline do Cloud Manager](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Etapa Código-fonte:

      - **Código para implantação**: Implantação direcionada
      - **Incluir**: Configuração
      - **Ambiente de implantação**: Nome do ambiente, por exemplo, wknd-program-dev.
      - **Repositório**: o repositório Git de onde o pipeline deve recuperar o código; por exemplo, `wknd-site`
      - **Ramificação Git**: o nome da ramificação do repositório Git.
      - **Localização do código**: `/config`, correspondente à pasta de configuração de nível superior criada na etapa anterior.

      ![Caixa de diálogo de configuração do pipeline do Cloud Manager](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Testar regras gerando tráfego

Para testar as regras, há várias ferramentas de terceiros disponíveis e sua organização pode ter uma ferramenta preferencial. Para fins de demonstração, vamos usar as seguintes ferramentas:

- [Curl](https://curl.se/) para testes básicos, como chamar um URL e verificar o código de resposta.

- [Vegeta](https://github.com/tsenart/vegeta) para executar o DOS (negação de serviço). Siga as instruções de instalação do [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

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

Depois de criar, implantar e testar as regras, é possível analisar os resultados usando **Elasticsearch, Logstash e Kibana (ELK)** ferramentas do painel. Ele pode analisar os logs de CDN do AEM CS, permitindo visualizar os resultados na forma de vários gráficos e tabelas.

As ferramentas do painel de controle podem ser clonadas diretamente do [Repositório GitHub da AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) e siga as etapas para instalar e carregar o **Regras de filtro de tráfego (incluindo WAF)** painel.

- Depois de carregar o painel de amostra, a página de ferramenta do painel Elástico deve ser semelhante ao seguinte:

  ![Painel de regras de filtro de tráfego ELK](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Como ainda não há logs CDN do AEMCS assimilados, o painel fica vazio.


## Próxima etapa

Saiba como declarar regras de filtro de tráfego, incluindo regras WAF na [Exemplos e análise de resultados](./examples-and-analysis.md) capítulo, uso do projeto AEM WKND Sites.
