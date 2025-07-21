---
title: Como configurar regras de filtro de tráfego, incluindo regras do WAF
description: Saiba como configurar o para criar, implantar, testar e analisar os resultados das regras de filtro de tráfego, incluindo regras do WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18306
thumbnail: null
exl-id: 0a738af8-666b-48dc-8187-9b7e6a8d7e1b
source-git-commit: b7f567da159865ff04cb7e9bd4dae0b140048e7d
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 14%

---

# Como configurar regras de filtro de tráfego, incluindo regras do WAF

Saiba **como configurar** regras de filtro de tráfego, incluindo regras do WAF (Firewall de Aplicativo Web). Neste tutorial, definimos a base para tutoriais subsequentes, em que você configurará e implantará regras, seguidas de testes e análise dos resultados.

Para demonstrar o processo de configuração, o tutorial usa o [Projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd).

>[!VIDEO](https://video.tv.adobe.com/v/3469395/?quality=12&learn=on)

## Visão geral da configuração

A base para tutoriais subsequentes envolve as seguintes etapas:

- _Criando regras_ em seu projeto do AEM na pasta `config`
- _Implantando regras_ usando o pipeline de configuração do Adobe Cloud Manager.
- _Testando regras_ com ferramentas como Curl, Vegeta e Nikto
- _Analisando resultados_ usando a Ferramenta de Análise de Log CDN do AEMCS

## Criar regras no seu projeto do AEM

Para definir as regras de filtro de tráfego do **padrão** e do **WAF** no seu projeto do AEM, siga estas etapas:

1. No nível superior do seu projeto do AEM, crie uma pasta chamada `config`.

2. Dentro da pasta `config`, crie um arquivo chamado `cdn.yaml`.

3. Usar a seguinte estrutura de metadados em `cdn.yaml`:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
```

![Arquivo e pasta de regras do projeto da WKND no AEM](./assets/setup/wknd-rules-file-and-folder.png)

No [próximo tutorial](#next-steps), você aprenderá a adicionar o **filtro de tráfego padrão recomendado e as regras do WAF** da Adobe ao arquivo acima, como uma base sólida para sua implementação.

## Implantar regras usando o Adobe Cloud Manager

Em preparação para implantar as regras, siga estas etapas:

1. Faça logon em [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) e selecione seu programa.

2. Na página **Visão geral do programa**, vá para o cartão **Pipelines** e clique em **+Adicionar** para criar um novo pipeline.

   ![Cartão de pipelines do Cloud Manager](./assets/setup/cloud-manager-pipelines-card.png)

3. No assistente de pipeline:

   - **Tipo**: pipeline de implantação
   - **Nome do pipeline**: Dev-Config

   ![Caixa de diálogo de configuração do pipeline do Cloud Manager](./assets/setup/cloud-manager-config-pipeline-step1-dialog.png)

4. Configuração do código Source:

   - **Código a ser implantado**: implantação direcionada
   - **Inclua**: Config
   - **Ambiente de Implantação**: por exemplo, `wknd-program-dev`
   - **Repositório**: repositório Git (por exemplo, `wknd-site`)
   - **Ramificação Git**: Sua ramificação de trabalho
   - **Local do Código**: `/config`

   ![Caixa de diálogo de configuração do pipeline do Cloud Manager](./assets/setup/cloud-manager-config-pipeline-step2-dialog.png)

5. Revise a configuração do pipeline e clique em **Salvar**.

No [próximo tutorial](#next-steps), você aprenderá a implantar o pipeline no seu ambiente do AEM.

## Testar regras usando ferramentas

Para testar a eficácia do filtro de tráfego padrão e das regras do WAF, você pode usar várias ferramentas para simular solicitações e analisar como suas regras respondem.

Verifique se você tem as seguintes ferramentas instaladas no computador local ou siga as instruções para instalá-las:

- [Curl](https://curl.se/): Testar fluxo de solicitação/resposta.
- [Vegeta](https://github.com/tsenart/vegeta): simular carga de solicitação alta (teste DoS).
- [Nikto](https://github.com/sullo/nikto/wiki): verificar vulnerabilidades.

Você pode verificar a instalação usando os seguintes comandos:

```shell
# Curl version check
$ curl --version

# Vegeta version check
$ vegeta -version

# Nikto version check
$ cd <PATH-OF-CLONED-REPO>/program
$ ./nikto.pl -Version
```

No [próximo tutorial](#next-steps), você aprenderá a usar essas ferramentas para simular altas cargas de solicitação e solicitações mal-intencionadas para testar a eficácia do seu filtro de tráfego e das regras do WAF.

## Analisar resultados

Para se preparar para analisar os resultados, siga estas etapas:

1. Instale a **Ferramenta de Análise de Log da CDN do AEMCS** para visualizar e analisar os padrões usando painéis pré-criados.

2. Execute a **assimilação de logs da CDN** baixando logs da interface do Cloud Manager. Como alternativa, encaminhe os logs diretamente para um destino de registro hospedado com suporte, como Splunk ou Elasticsearch.

### Ferramentas de análise de registros da CDN do AEM CS

Para analisar os resultados do filtro de tráfego e das regras do WAF, você pode usar a **Ferramenta de Análise de Log da CDN do AEMCS**. Essa ferramenta fornece painéis pré-criados para visualizar o tráfego de CDN e a atividade do WAF aproveitando os logs coletados do CDN do AEMCS.

A Ferramenta de Análise de Log CDN do AEMCS dá suporte a duas plataformas de observabilidade, **ELK** (Elasticsearch, Logstash, Kibana) e **Splunk**.

É possível usar o recurso de encaminhamento de logs para transmitir seus logs para um serviço de registro ELK ou Splunk hospedado, onde você pode instalar um painel para visualizar e analisar o filtro de tráfego padrão e as regras de filtro de tráfego do WAF. No entanto, para este tutorial, você configurará o painel em uma instância ELK local instalada em seu computador.

1. Clonar o repositório [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling).

2. Siga o [guia de configuração do contêiner ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md) para instalar e configurar a pilha ELK localmente.

3. Usando painéis ELK, você pode explorar métricas como solicitações de IP, tráfego bloqueado, padrões de URI e alertas de segurança.

   ![Painel de regras de filtro de tráfego ELK](./assets/setup/elk-dashboard.png)

>[!NOTE]
> 
> Se os logs ainda não tiverem sido assimilados do CDN do AEM CS, os painéis aparecerão vazios.

### Assimilação de logs CDN

Para assimilar logs CDN na pilha ELK, siga estas etapas:

- No cartão **Ambientes** do [Cloud Manager](https://my.cloudmanager.adobe.com/), baixe os logs de CDN do serviço AEMCS **Publish**.

  ![Downloads de logs da CDN do Cloud Manager](./assets/setup/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Pode levar até 5 minutos para que as novas solicitações apareçam nos logs da CDN.

- Copie o arquivo de log baixado (por exemplo, `publish_cdn_2025-06-06.log` na captura de tela abaixo) para a pasta `logs/dev` do projeto da ferramenta do painel “Elástico”.

  ![Pasta de logs da ferramenta ELK](./assets/setup/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Atualize a página da ferramenta do painel “Elástico”.
   - Na seção superior de **Filtro global**, edite o filtro `aem_env_name.keyword` e selecione o valor de ambiente `dev`.

     ![Filtro global da ferramenta ELK](./assets/setup/elk-tool-global-filter.png)

   - Para alterar o intervalo de tempo, clique no ícone de calendário, no canto superior direito, e selecione o intervalo de tempo desejado.

- No [próximo tutorial](#next-steps), você aprenderá a analisar os resultados do filtro de tráfego padrão e das regras do filtro de tráfego do WAF usando os painéis pré-criados na pilha ELK.

  ![Painéis pré-construídos da ferramenta ELK](./assets/setup/elk-tool-pre-built-dashboards.png)

## Resumo

Você criou com sucesso a base para a implementação das regras de filtro de tráfego, incluindo as regras do WAF no AEM as a Cloud Service. Você criou uma estrutura de arquivo de configuração, um pipeline para implantação e preparou ferramentas para testar e analisar os resultados.

## Próximas etapas

Saiba como implementar as regras recomendadas do Adobe usando os seguintes tutoriais:

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Proteção de sites da AEM usando regras padrão de filtro de tráfego" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Proteção de sites da AEM usando regras padrão de filtro de tráfego"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Proteção de sites da AEM usando regras padrão de filtro de tráfego">Protegendo sites da AEM usando regras de filtro de tráfego padrão</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra DoS, DDoS e abuso de bot usando regras de filtro de tráfego padrão recomendadas pela Adobe no AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aplicar regras</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Proteção de sites da AEM usando regras de filtro de tráfego do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Proteção de sites da AEM usando regras de filtro de tráfego do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Proteção de sites da AEM usando regras de filtro de tráfego do WAF">Protegendo sites da AEM usando as regras de filtro de tráfego do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando regras de filtro de tráfego do Firewall de aplicativo web (WAF) recomendadas pela Adobe no AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ativar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Casos de uso avançados

Além do filtro de tráfego padrão recomendado pela Adobe e das regras do WAF, é possível implementar cenários avançados para atender a requisitos específicos dos negócios. Esses cenários incluem:

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="Monitoramento de solicitações confidenciais" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Monitoramento de solicitações confidenciais"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Monitoramento de solicitações confidenciais">Monitoramento de solicitações confidenciais</a>
                    </p>
                    <p class="is-size-6">Saiba como monitorar solicitações confidenciais registrando-as usando regras de filtro de tráfego no AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="Restrição do acesso" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="Restrição do acesso"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="Restrição do acesso">Restringindo o acesso</a>
                    </p>
                    <p class="is-size-6">Saiba como restringir o acesso bloqueando solicitações específicas usando regras de filtro de tráfego no AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="Normalização de solicitações" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="Normalização de solicitações"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Normalização de solicitações">Normalizando solicitações</a>
                    </p>
                    <p class="is-size-6">Saiba como normalizar solicitações transformando-as usando regras de filtro de tráfego no AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Recursos adicionais

- [Regras De Filtro De Tráfego, Incluindo Regras Do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
