---
title: Bloquear ataques de DoS e DDoS por meio de regras de filtro de tráfego
description: Saiba como bloquear ataques de DoS e DDoS por meio de regras de filtro de tráfego na CDN fornecida pelo AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1924'
ht-degree: 100%

---

# Bloquear ataques de DoS e DDoS por meio de regras de filtro de tráfego

Saiba como bloquear ataques de negação de serviço (DoS) e de negação de serviço distribuída (DDoS) por meio de regras de **filtro de tráfego com limitação de taxa** e outras estratégias na CDN gerenciada pelo AEM as a Cloud Service (AEMCS). Esses ataques causam picos de tráfego na CDN e possivelmente no serviço de publicação do AEM (na origem) e podem afetar a capacidade de resposta e a disponibilidade dos sites.

Este tutorial serve como um guia de _como analisar os seus padrões de tráfego e configurar as [regras de filtro de tráfego](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ com limitação de taxa para mitigar esses ataques. O tutorial também descreve como [configurar alertas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts), para que você receba notificações quando houver suspeita de ataque.

## Entenda o que é proteção

Vamos entender as proteções contra DDoS padrão para o seu site do AEM:

- **Armazenamento em cache:** com boas políticas de armazenamento em cache, o impacto de um ataque de DDoS é mais limitado, porque a CDN impede que a maioria das solicitações atinja a origem e cause uma degradação do desempenho.
- **Dimensionamento automático:** os serviços de criação e publicação do AEM são dimensionados automaticamente para lidar com picos de tráfego, embora ainda possam ser afetados por grandes e súbitos aumentos de tráfego.
- **Bloqueio:** a CDN da Adobe bloqueará o tráfego para a origem se ele exceder uma taxa definida pela Adobe a partir de um endereço IP específico por PoP (ponto de presença) da CDN.
- **Alertas:** o centro de ações envia uma notificação de alerta de pico de tráfego na origem quando o tráfego excede uma taxa determinada. Esse alerta é acionado quando o tráfego para qualquer PoP da CDN excede uma taxa de solicitações _definida pela Adobe_ por endereço IP. Consulte [Alertas das regras de filtro de tráfego](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para obter mais detalhes.

Essas proteções integradas devem ser consideradas como uma linha de base da capacidade de uma organização de minimizar o impacto de um ataque de DDoS no desempenho. Como cada site tem características de desempenho diferentes e pode sofrer essa degradação do desempenho antes que o limite de taxa definido pela Adobe seja atingido, é recomendável estender as proteções padrão por meio da _configuração do cliente_.

Vamos analisar algumas medidas adicionais e recomendadas que os clientes podem tomar para proteger seus sites contra ataques de DDoS:

- Defina **regras de filtro de tráfego com limitação de taxa** para bloquear o tráfego que exceda uma determinada taxa em um mesmo endereço IP, por PoP. Normalmente, esse limite é inferior ao limite de taxa definido pela Adobe.
- Configure **alertas** para as regras de filtro de tráfego com limitação de taxa por meio de uma “ação de alerta”, para enviar uma notificação do centro de ações quando a regra for acionada.
- Aumente a cobertura do cache por definir **transformações de solicitação** que ignorem os parâmetros de consulta.

### Variações das regras de tráfego com limitação de taxa {#rate-limit-variations}

Há duas variações das regras de tráfego com limitação de taxa:

1. Borda: bloqueia solicitações com base na taxa de todo o tráfego (incluindo o que pode ser veiculado a partir do cache da CDN) de um determinado IP, por PoP.
1. Origem: bloqueia solicitações com base na taxa de tráfego destinada à origem para um determinado IP, por PoP.

## Jornada do cliente

As etapas abaixo refletem o provável processo que os clientes devem seguir para proteger seus sites.

1. Reconheça a necessidade de uma regra de filtro de tráfego com limitação de taxa. Isso pode decorrer do recebimento do alerta predefinido de pico de tráfego na origem da Adobe, ou pode ser uma decisão proativa para tomar precauções a fim de reduzir o risco de um DDoS bem-sucedido.
1. Analise os padrões de tráfego por meio de um painel, se o site já estiver ativo, para determinar os limites ideais para as suas regras de filtro de tráfego com limitação de taxa. Se o site ainda não estiver ativo, escolha valores com base no tráfego esperado.
1. Usando os valores da etapa anterior, configure as regras de filtro de tráfego com limitação de taxa. Habilite os alertas correspondentes para ser notificado sempre que o limite for atingido.
1. Receba alertas de regras de filtragem de tráfego sempre que ocorrerem picos de tráfego, os quais fornecem insights valiosos sobre se sua organização é um potencial alvo de agentes mal-intencionados.
1. Aja de acordo com o alerta, conforme necessário. Analise o tráfego para determinar se o pico reflete solicitações legítimas em vez de um ataque. Aumente os limites se o tráfego for legítimo; caso contrário, reduza-os.

O restante deste tutorial oferece orientações sobre esse processo.

## Reconhecer a necessidade de configurar regras {#recognize-the-need}

Como mencionado anteriormente, a Adobe bloqueia por padrão todoo tráfego na CDN que excede uma determinada taxa. No entanto, alguns sites podem apresentar diminuição de desempenho abaixo desse limite. Portanto, é necessário configurar regras de filtro de tráfego com limitação de taxa.

O ideal seria configurar as regras antes de entrar na etapa de produção. Na prática, muitas organizações definem regras apenas como resposta para alertas de picos de tráfego que indicam um provável ataque.

A Adobe envia um alerta de pico de tráfego na origem como uma [notificação do centro de ações](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/operations/actions-center) quando um limite padrão de tráfego de um único endereço IP é excedido para um determinado PoP. Se você recebeu esse alerta, é recomendável configurar uma regra de filtro de tráfego com limitação de taxa. Esse alerta padrão é diferente dos alertas que devem ser habilitados explicitamente pelos clientes ao definir as regras de filtro de tráfego, sobre as quais você aprenderá em uma seção futura.

## Análise de padrões de tráfego {#analyze-traffic}

Se o site já estiver ativo, você poderá analisar os padrões de tráfego usando logs da CDN e painéis fornecidos pela Adobe.

- **Painel de tráfego da CDN**: fornece insights sobre o tráfego por meio da taxa de solicitação da CDN e da origem, taxas de erro 4xx e 5xx e solicitações não armazenadas em cache. Também fornece o máximo de solicitações da CND e da origem por segundo e por endereço IP do cliente, além de insights para otimizar as configurações da CDN.

- **Taxa de ocorrência do cache da CDN**: fornece insights sobre a taxa de ocorrências do cache total e a contagem total de solicitações por status HIT, PASS e MISS. Também fornece os principais URLs de HIT, PASS e MISS.

Configure a ferramenta de painel usando _uma das seguintes opções_:

### ELK: configuração de ferramentas do painel

A ferramenta de painel **Elasticsearch, Logstash e Kibana (ELK)** fornecida pela Adobe pode ser usada para analisar os logs da CDN. Essa ferramenta inclui um painel que visualiza os padrões de tráfego, facilitando a determinação dos limites ideais para suas regras de filtro de tráfego com limitação de taxa.

- Clone o repositório do GitHub [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling).
- Configure a ferramenta seguindo as etapas [Como configurar o container ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container).
- Como parte da configuração, importe o arquivo `traffic-filter-rules-analysis-dashboard.ndjson` para visualizar os dados. O painel _Tráfego da CDN_ inclui visualizações que mostram o número máximo de solicitações por IP/POP na origem e na borda da CDN.
- No cartão [Ambientes _do_ Cloud Manager](https://my.cloudmanager.adobe.com/), baixe os logs da CDN do serviço de publicação do AEMCS.

  ![Downloads de logs da CDN do Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Pode levar até 5 minutos para que as novas solicitações apareçam nos logs da CDN.

### Splunk: configuração das ferramentas do painel

Clientes que têm o [encaminhamento do log do Splunk habilitado](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) podem criar novos painéis para analisar os padrões de tráfego.

Para criar painéis no Splunk, siga as etapas em [Painéis do Splunk para análise de log da CDN do AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

### Análise de dados

As seguintes visualizações estão disponíveis nos painéis ELK e Splunk:

- **RPS de borda por IP de cliente e POP**: esta visualização mostra o número máximo de solicitações por IP/POP **na borda da CDN**. O pico na visualização indica o número máximo de solicitações.

  **Painel ELK**:
  ![Painel ELK: máximo de solicitações por IP/POP](./assets/elk-edge-max-per-ip-pop.png)

  **Painel Splunk**:
  ![Painel Splunk: máximo de solicitações por IP/POP](./assets/splunk-edge-max-per-ip-pop.png)

- **RPS de origem por IP de cliente e POP**: esta visualização mostra o número máximo de solicitações por IP/POP **na origem**. O pico na visualização indica o número máximo de solicitações.

  **Painel ELK**:
  ![Painel ELK: máximo de solicitações de origem por IP/POP](./assets/elk-origin-max-per-ip-pop.png)

  **Painel Splunk**:
  ![Painel Splunk: máximo de solicitações de origem por IP/POP](./assets/splunk-origin-max-per-ip-pop.png)

## Escolha de valores-limite

Os valores-limite para regras de filtro de tráfego de limite de taxa devem ser baseados na análise acima, a fim de garantir que o tráfego legítimo não seja bloqueado. Consulte a tabela a seguir para obter orientações sobre como escolher os valores de limite:

| Variação | Valor |
| :--------- | :------- |
| Origem | Use o valor mais alto do máximo de solicitações de origem por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |
| Borda | Use o valor mais alto do máximo de solicitações de borda por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |

O múltiplo a ser usado depende das suas expectativas de picos normais do tráfego devido ao tráfego orgânico, campanhas e outros eventos. Um múltiplo entre 5 e 10 pode ser razoável.

Se o seu site ainda não estiver ativo, não haverá dados para analisar, e você deverá fazer um palpite informado dos valores apropriados a serem definidos para as regras de filtro de tráfego com limitação de taxa. Por exemplo:

| Variação | Valor |
|------------------------------ |:-----------:|
| Borda | 500 |
| Origem | 100 |

## Configuração de regras {#configure-rules}

Configure as regras de **filtro de tráfego com limitação de taxa** no arquivo `/config/cdn.yaml` do seu projeto do AEM, com valores baseados nas informações acima. Se necessário, consulte a sua equipe de segurança da web para verificar se os valores de limite de taxa são apropriados e não bloqueiam o tráfego legítimo.

Consulte [Criar regras no seu projeto do AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) para obter mais detalhes.

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
    ...
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
```

Observe que as regras de origem e de borda são declaradas, e que a propriedade de alertas está definida como `true`, para que você possa receber alertas sempre que o limite for atingido, o que provavelmente indica um ataque.

É recomendado configurar o tipo de ação para o registro inicial, para que você possa monitorar o tráfego por algumas horas ou dias, garantindo que o tráfego legítimo não exceda essas taxas. Após alguns dias, altere para o modo de bloqueio.

Siga as etapas abaixo para implantar as alterações no ambiente do AEMCS:

- Confirme e envie as alterações acima ao seu repositório Git do Cloud Manager.
- Implante as alterações no ambiente do AEMCS por meio do pipeline de configuração do Cloud Manager. Consulte [Implantar regras por meio do Cloud Manager](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) para obter mais detalhes.
- Para verificar se a **regra de filtro de tráfego com limitação de taxa** está funcionando como esperado, simule um ataque conforme descrito na seção [Simulação de ataques](#attack-simulation). Limite o número de solicitações a um valor maior que o valor limite da taxa definido na regra.

### Configurar regras de transformação de solicitação {#configure-request-transform-rules}

Além das regras de filtro de tráfego com limitação de taxa, é recomendável usar [transformações de solicitação](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) para desfazer a definição de parâmetros de consulta não necessários para o aplicativo, a fim de minimizar as formas de ignorar o cache por meio de técnicas de eliminação de cache. Por exemplo, se você quiser permitir apenas os parâmetros de consulta `search` e `campaignId`, defina a seguinte regra:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  requestTransformations:
    rules:
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Receber alertas das regras de filtro de tráfego {#receiving-alerts}

Como mencionado acima, se a regra de filtro de tráfego incluir *alerta: verdadeiro*, um alerta será recebido quando a regra for correspondida.

## Agir com base em alertas {#acting-on-alerts}

Às vezes, o alerta é informativo e fornece uma ideia sobre a frequência dos ataques. Vale a pena analisar os seus dados da CDN no painel descrito acima para confirmar se o pico de tráfego se deve a um ataque ou apenas a um aumento no volume do tráfego legítimo. Neste último caso, considere aumentar o limite.

## Simulação de ataques{#attack-simulation}

Esta seção descreve métodos para simular um ataque de DoS a fim de gerar dados para os painéis usados neste tutorial e para confirmar se as regras configuradas estão bloqueando os ataques com sucesso.

>[!CAUTION]
>
> Não siga estas etapas em um ambiente de produção. As etapas a seguir são somente para fins de simulação.
>
>Se você tiver recebido um alerta sobre um pico no tráfego, prossiga para a seção [Analisar padrões de tráfego](#analyzing-traffic-patterns).

Para simular um ataque, é possível usar ferramentas como o [Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html), o [Apache JMeter](https://jmeter.apache.org/), o [Vegeta](https://github.com/tsenart/vegeta), entre outras.

### Solicitações de borda

Usando o seguinte comando do [Vegeta](https://github.com/tsenart/vegeta), é possível fazer muitas solicitações para o site:

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=60s | vegeta report
```

O comando acima faz 120 solicitações em 5 segundos e gera um relatório. Se o site não tiver uma limitação de taxa, isso poderá causar um pico no tráfego.

### Solicitações de origem

Para ignorar o cache da CDN e fazer solicitações à origem (serviço de publicação do AEM), adicione um parâmetro de consulta exclusivo no URL. Consulte o exemplo de script do Apache JMeter em [Simular ataque de DoS com o script do JMeter](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

