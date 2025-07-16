---
title: Proteção de sites da AEM usando regras padrão de filtro de tráfego
description: Saiba como proteger sites da AEM contra DoS (Negação de serviço) e abuso de bot usando regras de filtro de tráfego padrão recomendadas pela Adobe no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18307
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1780'
ht-degree: 7%

---


# Proteção de sites da AEM usando regras padrão de filtro de tráfego

Saiba como proteger sites da AEM contra DoS (Negação de Serviço), DDoS (Negação de Serviço Distribuída) e abuso de bot usando _regras recomendadas pela Adobe_ **regras padrão de filtro de tráfego** no AEM as a Cloud Service.

## Objetivos de aprendizagem

- Revise as regras de filtro de tráfego padrão recomendadas pela Adobe.
- Defina, implante, teste e analise os resultados das regras.
- Entenda quando e como refinar as regras com base nos padrões de tráfego.
- Saiba como usar o Centro de ações da AEM para revisar alertas gerados pelas regras.

### Visão geral da implementação

As etapas de implementação incluem:

- Adicionar as regras de filtro de tráfego padrão ao arquivo `/config/cdn.yaml` do projeto WKND do AEM.
- Confirmar e enviar as alterações para o repositório Git do Cloud Manager.
- Implantar as alterações no ambiente do AEM usando o pipeline de configuração do Cloud Manager.
- Testando as regras por simulação de ataque de DoS usando [Vegeta](https://github.com/tsenart/vegeta)
- Analisar os resultados usando os logs de CDN do AEM CS e a ferramenta do painel ELK.

## Pré-requisitos

Antes de continuar, verifique se você concluiu o trabalho de base necessário, conforme descrito no [tutorial Como configurar o filtro de tráfego e as regras do WAF](../setup.md). Além disso, você clonou e implantou o [Projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) no seu ambiente AEM.

## Ações principais das regras

Antes de analisarmos os detalhes das regras padrão de filtro de tráfego, vamos entender as principais ações que essas regras executam. O atributo `action` em cada regra define como o filtro de tráfego deve responder quando as condições forem atendidas. As ações incluem:

- **Log**: as regras registram os eventos para monitoramento e análise, permitindo que você revise padrões de tráfego e ajuste limites conforme necessário. Ele é especificado pelo atributo `type: log`.

- **Alerta**: as regras acionam alertas quando as condições são atendidas, ajudando você a identificar possíveis problemas. Ele é especificado pelo atributo `alert: true`.

- **Bloquear**: as regras bloqueiam o tráfego quando as condições são atendidas, impedindo o acesso ao site do AEM. Ele é especificado pelo atributo `action: block`.

## Revisar e definir regras

As regras de filtro de tráfego padrão recomendadas pela Adobe servem como uma camada fundamental para identificar padrões de tráfego potencialmente mal-intencionados, registrando eventos como limites de taxa baseados em IP que estão sendo excedidos e bloqueando o tráfego de países específicos. Esses logs ajudam as equipes a validar limites e tomar decisões informadas para a **transição para as regras de modo de bloqueio** sem interromper o tráfego legítimo.

Vamos revisar as três regras padrão de filtro de tráfego que você deve adicionar ao arquivo `/config/cdn.yaml` do projeto WKND do AEM:

- **Impedir DoS na Edge**: esta regra detecta possíveis ataques de Negação de Serviço (DoS) na borda da CDN, monitorando solicitações por segundo (RPS) de IPs clientes.
- **Impedir DoS na Origem**: esta regra detecta possíveis ataques de Negação de Serviço (DoS) na origem por meio do monitoramento de solicitações de busca de IPs clientes.
- **Bloquear Países do OFAC**: esta regra bloqueia o acesso de países específicos que estejam sob restrições do OFAC (Office of Foreign Assets Control).

### &#x200B;1. Impedir o DoS no Edge

Esta regra **envia um alerta** quando detecta um possível ataque de Negação de Serviço (DoS) na CDN. Os critérios para acionar essa regra são quando um cliente excede **500 solicitações por segundo** (em média, mais de 10 segundos) por POP (Ponto de Presença) de CDN na borda.

Conta **todas** as solicitações e as agrupa por IP do cliente.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: prevent-dos-attacks-edge
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 500
        window: 10
        penalty: 300
        count: all
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

O atributo `action` especifica que a regra deve registrar os eventos e acionar um alerta quando as condições forem atendidas. Dessa forma, ele ajuda a monitorar possíveis ataques de DoS sem bloquear o tráfego legítimo. No entanto, seu objetivo é eventualmente fazer a transição dessa regra para o modo de bloqueio depois de validar os padrões de tráfego e ajustar os limites.

### &#x200B;2. Evitar o DoS na origem

Esta regra **envia um alerta** quando detecta um possível ataque de Negação de Serviço (DoS) na origem. Os critérios para acionar esta regra são quando um cliente excede **100 solicitações por segundo** (em média, mais de 10 segundos) por IP de cliente na origem.

Conta **buscas** (solicitações de ignorar cache) e as agrupa por IP do cliente.

```yaml
...
    - name: prevent-dos-attacks-origin
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 100
        window: 10
        penalty: 300
        count: fetches
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

O atributo `action` especifica que a regra deve registrar os eventos e acionar um alerta quando as condições forem atendidas. Dessa forma, ele ajuda a monitorar possíveis ataques de DoS sem bloquear o tráfego legítimo. No entanto, seu objetivo é eventualmente fazer a transição dessa regra para o modo de bloqueio depois de validar os padrões de tráfego e ajustar os limites.

### &#x200B;3. Bloquear países OFAC

Esta regra bloqueia o acesso de países específicos que se enquadram nas restrições [OFAC](https://ofac.treasury.gov/sanctions-programs-and-country-information).
Você pode revisar e modificar a lista de países conforme necessário.

```yaml
...
    - name: block-ofac-countries
      when:
        allOf:
          - { reqProperty: tier, in: ["author", "publish"] }
          - reqProperty: clientCountry
            in:
              - SY
              - BY
              - MM
              - KP
              - IQ
              - CD
              - SD
              - IR
              - LR
              - ZW
              - CU
              - CI
      action: block
```

O atributo `action` especifica que a regra deve bloquear o acesso dos países especificados. Isso ajuda a impedir o acesso ao site do AEM de regiões que podem representar riscos de segurança.

O arquivo `cdn.yaml` completo com as regras acima tem esta aparência:

![Regras YAML da WKND CDN](../assets/use-cases/wknd-cdn-yaml-rules.png)

## Implantar regras

Para implantar as regras acima, siga estas etapas:

- Confirme e envie as alterações ao repositório do Git do Cloud Manager.

- Implante as alterações no ambiente do AEM usando o pipeline de configuração do Cloud Manager [criado anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

  ![Pipeline de configuração do Cloud Manager](../assets/use-cases/cloud-manager-config-pipeline.png)

## Testar regras

Para verificar a eficácia das regras de filtro de tráfego padrão, na **Edge da CDN** e na **Origem**, simule um alto tráfego de solicitação usando a [Vegeta](https://github.com/tsenart/vegeta), uma ferramenta versátil de teste de carga HTTP.

- Testar a regra de DoS no Edge (limite de 500 rps). O comando a seguir simula 200 solicitações por segundo durante 15 segundos, o que excede o limite do Edge (500 rps).

  ```shell
  $echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html" | vegeta attack -rate=200 -duration=15s | vegeta report
  ```

  ![Edge de Ataque de DoS Vegeta](../assets/use-cases/vegeta-dos-attack-edge.png)

  >[!IMPORTANT]
  >
  >  Observe os códigos de Sucesso *100%* e Status _200_ no relatório acima. Como as regras estão definidas como `log` e `alert`, as solicitações _não estão bloqueadas_, mas estão registradas para fins de monitoramento, análise e alerta.

- Testar a regra de DoS na origem (limite de 100 rps). O comando a seguir simula 110 solicitações de busca por segundo por 1 segundo, o que excede o limite Origin (100 rps). Para simular solicitações de desvio de cache, o arquivo `targets.txt` é criado com parâmetros de consulta exclusivos para garantir que cada solicitação seja tratada como uma solicitação de busca.

  ```shell
  # Create targets.txt with unique query parameters
  $for i in {1..110}; do
    echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html?ts=$(date +%s)$i"
  done > targets.txt
  
  # Use the targets.txt file to simulate fetch requests
  $vegeta attack -rate=110 -duration=1s -targets=targets.txt | vegeta report
  ```

  ![Origem de Ataque Vegeta DoS](../assets/use-cases/vegeta-dos-attack-origin.png)

  >[!IMPORTANT]
  >
  >  Observe os códigos de Sucesso *100%* e Status _200_ no relatório acima. Como as regras estão definidas como `log` e `alert`, as solicitações _não estão bloqueadas_, mas estão registradas para fins de monitoramento, análise e alerta.

- Para fins de simplicidade, a regra OFAC não é testada aqui.

## Revisar alertas

Os alertas são gerados quando as regras de filtro de tráfego são acionadas. Você pode examinar esses alertas no [Centro de Ações da AEM](https://experience.adobe.com/aem/actions-center).

![Centro de Ações do WKND AEM](../assets/use-cases/wknd-aem-action-center.png)

## Analisar resultados

Para analisar os resultados das regras de filtro de tráfego, você pode usar os logs de CDN do AEM CS e a ferramenta de painel ELK. Siga as instruções da seção de configuração [Assimilação de logs CDN](../setup.md#ingest-cdn-logs) para assimilar os logs CDN na pilha ELK.

Na captura de tela a seguir, você pode ver os logs de CDN do ambiente de desenvolvimento do AEM assimilados na pilha de ELK.

![ELK de Logs CDN WKND](../assets/use-cases/wknd-cdn-logs-elk.png)

Dentro do aplicativo ELK, o **Painel de Tráfego da CDN** deve mostrar o pico no **Edge** e **Origin** durante os ataques simulados de DoS.

Os dois painéis, _Edge RPS por IP de Cliente e POP_ e _RPS de Origem por IP de Cliente e POP_, exibem as solicitações por segundo (RPS) na borda e na origem, respectivamente, agrupadas por IP de Cliente e Ponto de Presença (POP).

![Painel de Tráfego do Edge da WKND CDN](../assets/use-cases/wknd-cdn-edge-traffic-dashboard.png)

Você também pode usar outros painéis no Painel de Tráfego da CDN para analisar os padrões de tráfego, como _Principais IPs de Clientes_, _Principais Países_ e _Principais Agentes de Usuários_. Esses painéis o ajudam a identificar possíveis ameaças e ajustar as regras do filtro de tráfego de acordo.

### Integração do Splunk

Clientes que têm o [encaminhamento do log do Splunk habilitado](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) podem criar novos painéis para analisar os padrões de tráfego.

Para criar painéis no Splunk, siga as etapas em [Painéis do Splunk para análise de log da CDN do AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

A captura de tela a seguir mostra um exemplo de um painel do Splunk que exibe o máximo de solicitações de origem e borda por IP, o que pode ajudar você a identificar possíveis ataques de DoS.

![Painel do Splunk - Máximo de solicitações de origem e borda por IP](../assets/use-cases/splunk-dashboard-max-origin-edge-requests.png)

## Quando e como refinar regras

Seu objetivo é evitar o bloqueio de tráfego legítimo e, ao mesmo tempo, proteger o site do AEM contra possíveis ameaças. As regras padrão de filtro de tráfego são projetadas para alertar e registrar (e eventualmente bloquear quando o modo é alternado) ameaças sem bloquear o tráfego legítimo.

Para refinar as regras, considere as seguintes etapas:

- **Monitorar padrões de tráfego**: Use os logs CDN e o painel ELK para monitorar padrões de tráfego e identificar quaisquer anomalias ou picos no tráfego.
- **Ajustar limites**: com base nos padrões de tráfego, ajuste os limites (aumente ou diminua os limites de taxa) nas regras para melhor se adequar aos seus requisitos específicos. Por exemplo, se você observar que o tráfego legítimo acionou os alertas, poderá aumentar os limites de taxa ou ajustar os agrupamentos.
A tabela a seguir fornece orientação sobre como escolher os valores de limite:

  | Variação | Valor |
  | :--------- | :------- |
  | Origem | Use o valor mais alto do máximo de solicitações de origem por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |
  | Borda | Use o valor mais alto do máximo de solicitações de borda por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |

  Consulte também a seção [Escolhendo valores limite](../../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) para obter mais detalhes.

- **Mover para regras de bloqueio**: depois de validar os padrões de tráfego e ajustar os limites, você deve fazer a transição das regras para o modo de bloqueio.

## Resumo

Neste tutorial, você aprendeu a proteger os sites da AEM contra DoS (Negação de serviço), DDoS (Negação de serviço distribuída) e abuso de bot usando regras de filtro de tráfego padrão recomendadas pela Adobe no AEM as a Cloud Service.

## Regras recomendadas do WAF

Adobe Saiba como implementar as regras recomendadas pela WAF para proteger seus sites da AEM contra ameaças sofisticadas que usam técnicas avançadas para ignorar as medidas de segurança tradicionais.

<!-- CARDS
{target = _self}

* ./using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ../assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./using-waf-rules.md" title="Proteção de sites da AEM usando regras de filtro de tráfego do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/use-cases/using-waf-rules.png" alt="Proteção de sites da AEM usando regras de filtro de tráfego do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./using-waf-rules.md" target="_self" rel="referrer" title="Proteção de sites da AEM usando regras de filtro de tráfego do WAF">Protegendo sites da AEM usando as regras de filtro de tráfego do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando regras de filtro de tráfego do Firewall de aplicativo web (WAF) recomendadas pela Adobe no AEM as a Cloud Service.</p>
                </div>
                <a href="./using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ativar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Casos de uso - além das regras padrão

Para cenários mais avançados, você pode explorar os seguintes casos de uso que demonstram como implementar regras de filtro de tráfego personalizadas com base em requisitos de negócios específicos:

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="Monitoramento de solicitações confidenciais" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="Monitoramento de solicitações confidenciais"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="Monitoramento de solicitações confidenciais">Monitoramento de solicitações confidenciais</a>
                    </p>
                    <p class="is-size-6">Saiba como monitorar solicitações confidenciais registrando-as usando regras de filtro de tráfego no AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="Restrição do acesso" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="Restrição do acesso"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="Restrição do acesso">Restringindo o acesso</a>
                    </p>
                    <p class="is-size-6">Saiba como restringir o acesso bloqueando solicitações específicas usando regras de filtro de tráfego no AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="Normalização de solicitações" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="Normalização de solicitações"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="Normalização de solicitações">Normalizando solicitações</a>
                    </p>
                    <p class="is-size-6">Saiba como normalizar solicitações transformando-as usando regras de filtro de tráfego no AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Recursos adicionais

- [Regras de início recomendadas](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)


