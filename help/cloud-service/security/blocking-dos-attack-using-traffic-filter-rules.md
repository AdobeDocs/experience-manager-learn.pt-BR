---
title: Bloqueio de ataques de DoS e DDoS usando regras de filtro de tráfego
description: Saiba como bloquear ataques de DoS e DDoS usando regras de filtro de tráfego no CDN fornecido pelo AEM as a Cloud Service.
version: Cloud Service
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
source-git-commit: c7c78ca56c1d72f13d2dc80229a10704ab0f14ab
workflow-type: tm+mt
source-wordcount: '1968'
ht-degree: 0%

---

# Bloqueio de ataques de DoS e DDoS usando regras de filtro de tráfego

Saiba como bloquear ataques de Negação de serviço (DoS) e de Negação de serviço distribuída (DDoS) usando **filtro de tráfego de limite de taxa** regras e outras estratégias na CDN gerenciada pelo AEM as a Cloud Service (AEMCS). Esses ataques causam picos de tráfego no CDN e possivelmente no serviço de publicação do AEM (também conhecido como origem) e podem afetar a capacidade de resposta e a disponibilidade do site.

Este tutorial serve como um guia sobre _como analisar seus padrões de tráfego e configurar o limite de taxa [regras de filtro de tráfego](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ para mitigar esses ataques. O tutorial também descreve como [configurar alertas](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para ser notificado quando houver suspeita de ataque.

## Noções básicas sobre proteção

Vamos entender as proteções de DDoS padrão para o seu site de AEM:

- **Armazenamento em cache:** Com boas políticas de armazenamento em cache, o impacto de um ataque de DDoS é mais limitado porque a CDN impede que a maioria das solicitações vá para a origem e cause degradação de desempenho.
- **Dimensionamento automático:** Os serviços de autoria e publicação do AEM são dimensionados automaticamente para lidar com picos de tráfego, embora ainda possam ser afetados por aumentos súbitos e maciços no tráfego.
- **Bloqueio:** O CDN de Adobe bloqueia o tráfego para a origem se ele exceder uma taxa definida por Adobe de um endereço IP específico, por PoP (Ponto de Presença) de CDN.
- **Alerta:** O Centro de Ações envia uma notificação de alerta de pico de tráfego na origem quando o tráfego excede uma determinada taxa. Esse alerta é disparado quando o tráfego para qualquer PoP de CDN excede um _definido por Adobe_ taxa de solicitação por endereço IP. Consulte [Alertas de regras de filtro de tráfego](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para obter mais detalhes.

Essas proteções integradas devem ser consideradas uma linha de base para a capacidade de uma organização de minimizar o impacto no desempenho de um ataque de DDoS. Como cada site tem características de desempenho diferentes e pode observar que a degradação de desempenho antes do limite de taxa definido pelo Adobe é atendida, é recomendável estender as proteções padrão por meio do _configuração do cliente_.

Vamos analisar algumas medidas adicionais e recomendadas que os clientes podem tomar para proteger seus sites contra ataques de DDoS:

- Declarar **regras de filtro de tráfego de limite de taxa** para bloquear o tráfego que excede uma determinada taxa de um único endereço IP por PoP. Normalmente, esses são um limite mais baixo do que o limite de taxa definido pelo Adobe.
- Configurar **alertas** nas regras de filtro de tráfego de limite de taxa por meio de uma &quot;ação de alerta&quot;, para que, quando a regra for acionada, uma notificação da Central de ações seja enviada.
- Aumentar a cobertura do cache declarando **solicitar transformações** para ignorar parâmetros de consulta.

>[!NOTE]
>
>A variável [alertas de regra de filtro de tráfego](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) O recurso ainda não foi lançado. Para obter acesso por meio do programa de adoção antecipada, envie um email para **<aemcs-waf-adopter@adobe.com>**.

### Variações das regras de tráfego de limite de taxa {#rate-limit-variations}

Há duas variações de regras de tráfego de limite de taxa:

1. Edge - bloqueia solicitações com base na taxa de todo o tráfego (incluindo o que pode ser veiculado a partir do cache CDN) por PoP para um determinado IP.
1. Origem - bloqueia solicitações com base na taxa de tráfego destinada à origem, para um determinado IP, por PoP.

## Jornada do cliente

As etapas abaixo refletem o processo provável pelo qual os clientes devem proteger seus sites.

1. Reconheça a necessidade de uma regra de filtro de tráfego de limite de taxa. Isso pode ser o resultado do recebimento do pico de tráfego de saída de Adobe no alerta de origem, ou pode ser uma decisão proativa para tomar precauções a fim de reduzir o risco de um DDoS bem-sucedido.
1. Analise os padrões de tráfego usando um painel, se o site já estiver ativo, para determinar os limites ideais para suas regras de filtro de tráfego de limite de taxa. Se o site ainda não estiver ativo, escolha valores com base nas expectativas de tráfego.
1. Usando os valores da etapa anterior, configure as regras de filtro de tráfego de limite de taxa. Ative os alertas correspondentes para ser notificado sempre que o limite for atingido.
1. Receba alertas de regras de filtro de tráfego sempre que picos de tráfego ocorrerem, fornecendo informações valiosas sobre se sua organização está sendo potencialmente alvo de agentes mal-intencionados.
1. Aja sobre o alerta, conforme necessário. Analise o tráfego para determinar se o pico reflete solicitações legítimas em vez de um ataque. Aumente os limites se o tráfego for legítimo, ou diminua-os se não for.

O restante deste tutorial o orienta por esse processo.

## Reconhecendo a necessidade de configurar regras {#recognize-the-need}

Como mencionado anteriormente, o Adobe por padrão bloqueia o tráfego na CDN que excede uma determinada taxa. No entanto, alguns sites podem enfrentar desempenho degradado abaixo desse limite. Portanto, as regras de filtro de tráfego de limite de taxa devem ser configuradas.

Idealmente, você configuraria as regras antes de entrar em produção. Na prática, muitas organizações declaram regras reativamente apenas uma vez alertadas de um pico de tráfego, indicando um provável ataque.

Adobe envia um pico de tráfego no alerta de origem como um [Notificação da Central de Ações](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/actions-center) quando um limite padrão de tráfego de um único endereço IP é excedido, para um determinado PoP. Se você recebeu esse alerta, é recomendável configurar uma regra de filtro de tráfego de limite de taxa. Esse alerta padrão é diferente dos alertas que devem ser ativados explicitamente pelos clientes ao definir as regras de filtro de tráfego, sobre as quais você aprenderá em uma seção futura.


## Análise de padrões de tráfego {#analyze-traffic}

Se o site já estiver ativo, você poderá analisar os padrões de tráfego usando logs CDN e painéis fornecidos pelo Adobe.

- **Painel de tráfego CDN**: fornece insights sobre o tráfego por meio da taxa de solicitação de CDN e Origem, taxas de erro 4xx e 5xx e solicitações não armazenadas em cache. Também fornece o máximo de solicitações CND e Origin por segundo por endereço IP de cliente e mais insights para otimizar as configurações de CDN.

- **Taxa de acertos do cache do CDN**: fornece insights sobre a taxa de acertos do cache total e a contagem total de solicitações por status HIT, PASS e MISS. Também fornece os principais URLs de HIT, PASS e MISS.

Configurar as ferramentas do painel usando _uma das seguintes opções_:

### ELK - configurando ferramentas de painel de controle

A variável **Elasticsearch, Logstash e Kibana (ELK)** As ferramentas de painel fornecidas pelo Adobe podem ser usadas para analisar os logs de CDN. Essa ferramenta inclui um painel que visualiza os padrões de tráfego, facilitando a determinação dos limites ideais para suas regras de filtro de tráfego de limite de taxa.

- Clonar o [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) Repositório GitHub.
- Configure a ferramenta seguindo o método [Como configurar o contêiner ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container) etapas.
- Como parte da configuração, importe o `traffic-filter-rules-analysis-dashboard.ndjson` arquivo para visualizar os dados. A variável _Tráfego CDN_ O painel inclui visualizações que mostram o número máximo de solicitações por IP/POP na borda e na origem do CDN.
- No [Cloud Manager](https://my.cloudmanager.adobe.com/)do _Ambientes_ , baixe os logs de CDN do serviço de publicação do AEMCS.

  ![Downloads de logs do CDN do Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Pode levar até 5 minutos para que as novas solicitações apareçam nos logs de CDN.

### Splunk - configurando as ferramentas do painel

Clientes que têm [Encaminhamento do Log do Splunk habilitado](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) O pode criar novos painéis para analisar os padrões de tráfego.

Para criar painéis no Splunk, siga [Painéis do Splunk para a Análise de Log do AEM CS CDN](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/READEME.md#splunk-dashboards-for-aemcs-cdn-log-analysis) etapas.

### Examinando dados

As seguintes visualizações estão disponíveis nos painéis ELK e Splunk:

- **RPS de borda por IP e POP do cliente**: esta visualização mostra o número máximo de solicitações por IP/POP **no CDN Edge**. O pico na visualização indica o número máximo de solicitações.

  **Painel ELK**:
  ![Painel ELK - Máximo de solicitações por IP/POP](./assets/elk-edge-max-per-ip-pop.png)

  **Painel do Splunk**:\
  ![Painel do Splunk - Máximo de Solicitações por IP/POP](./assets/splunk-edge-max-per-ip-pop.png)

- **RPS de origem por IP e POP do cliente**: esta visualização mostra o número máximo de solicitações por IP/POP **na origem**. O pico na visualização indica o número máximo de solicitações.

  **Painel ELK**:
  ![Painel ELK - Máximo de solicitações de origem por IP/POP](./assets/elk-origin-max-per-ip-pop.png)

  **Painel do Splunk**:
  ![Painel do Splunk - Máximo de solicitações de origem por IP/POP](./assets/splunk-origin-max-per-ip-pop.png)

## Escolha de valores de limite

Os valores-limite para as regras de filtro de tráfego limite de taxa devem se basear na análise acima e garantir que o tráfego legítimo não seja bloqueado. Consulte a tabela a seguir para obter orientação sobre como escolher os valores de limite:

| Variação | Valor |
| :--------- | :------- |
| Origem | Use o valor mais alto do Máximo de solicitações de origem por IP/POP em **normal** condições de tráfego (ou seja, não a taxa no momento de um DDoS) e aumentá-la em um múltiplo |
| Edge | Use o valor mais alto do Máximo de solicitações de borda por IP/POP em **normal** condições de tráfego (ou seja, não a taxa no momento de um DDoS) e aumentá-la em um múltiplo |

O múltiplo a ser usado depende de suas expectativas de picos normais no tráfego devido ao tráfego orgânico, campanhas e outros eventos. Um múltiplo entre 5-10 pode ser razoável.

Se o site ainda não estiver ativo, não há dados para analisar e você deve fazer uma suposição detalhada sobre os valores apropriados a serem definidos para as regras de filtro de tráfego de limite de taxa. Por exemplo:

| Variação | Valor |
|------------------------------ |:-----------:|
| Edge | 500 |
| Origem | 100 |

## Configuração de regras {#configure-rules}

Configure o **filtro de tráfego de limite de taxa** regras no do projeto AEM `/config/cdn.yaml` arquivo, com valores baseados na discussão acima. Se necessário, consulte sua equipe de Segurança da Web para verificar se os valores de limite de taxa são apropriados e não bloqueiam o tráfego legítimo.

Consulte [Criar regras no projeto AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) para obter mais detalhes.

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
          experimental_alert: true
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
          experimental_alert: true   
          
```

Observe que as regras de origem e de borda são declaradas e que a propriedade de alerta está definida como `true` assim, você pode receber alertas sempre que o limite for atingido, provavelmente indicando um ataque.

>[!NOTE]
>
>A variável _experimental_ prefix_ na frente de experimental_alert será removido quando o recurso de alerta for lançado. Para participar do programa de adoção antecipada, envie um email para **<aemcs-waf-adopter@adobe.com>**.

Recomenda-se que o tipo de ação seja definido para registrar inicialmente, para que você possa monitorar o tráfego por algumas horas ou dias, garantindo que o tráfego legítimo não exceda essas taxas. Após alguns dias, altere para o modo de bloqueio.

Siga as etapas abaixo para implantar as alterações no ambiente do AEM CS:

- Confirme e envie as alterações acima para o repositório Git do Cloud Manager.
- Implante as alterações no ambiente do AEM CS usando o pipeline de configuração do Cloud Manager. Consultar [Implantar regras por meio do Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) para obter mais detalhes.
- Para verificar a **regra de filtro de tráfego de limite de taxa** estiver funcionando como esperado, é possível simular um ataque conforme descrito na seção [Simulação de ataque](#attack-simulation) seção. Limite o número de solicitações a um valor maior que o valor limite da taxa definido na regra.

### Configurar regras de transformação de solicitação {#configure-request-transform-rules}

Além das regras de filtro de tráfego de limite de taxa, é recomendável usar [solicitar transformações](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) para desfazer a definição de parâmetros de consulta não necessários para a aplicação minimizar as maneiras de ignorar o cache por meio de técnicas de eliminação de cache. Por exemplo, se você só quiser permitir `search` e `campaignId` parâmetros de consulta, a seguinte regra pode ser declarada:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: 
    - dev
    - stage
    - prod  
data:  
  experimental_requestTransformations:
    rules:            
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Recebimento de alertas de regras de filtro de tráfego {#receiving-alerts}

Como mencionado acima, se a regra de filtro de tráfego incluir *experimental_alert: true*, um alerta é recebido quando a regra é correspondida.

## Atuação em alertas {#acting-on-alerts}

Às vezes, o alerta é informativo, o que dá uma ideia da frequência dos ataques. Vale a pena analisar seus dados de CDN usando o painel descrito acima, para validar se o pico de tráfego se deve a um ataque e não apenas a um aumento no volume de tráfego legítimo. No último caso, considere aumentar o limite.

## Simulação de ataque{#attack-simulation}

Esta seção descreve métodos para simular um ataque de DoS, que podem ser usados para gerar dados para os painéis usados neste tutorial e para validar se as regras configuradas bloqueiam os ataques com êxito.

>[!CAUTION]
>
> Não execute essas etapas em um ambiente de produção. As etapas a seguir são somente para fins de simulação.
> 
>Se você recebeu um alerta indicando um pico no tráfego, prossiga para a [Análise de padrões de tráfego](#analyzing-traffic-patterns) seção.

Para simular um ataque, ferramentas como [Benchmark Apache](https://httpd.apache.org/docs/2.4/programs/ab.html), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta), e outros podem ser usados.

### Solicitações de borda

Usando o seguinte [Vegeta](https://github.com/tsenart/vegeta) comando, você pode fazer muitas solicitações ao seu site:

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=5s | vegeta report
```

O comando acima faz 120 solicitações por 5 segundos e gera um relatório. Supondo que o site não tenha taxa limitada, isso pode causar um pico no tráfego.

### Solicitações de origem

Para ignorar o cache do CDN e fazer solicitações à origem (serviço de Publicação AEM), você pode adicionar um parâmetro de consulta exclusivo ao URL. Consulte a amostra de script Apache JMeter do [Simular um ataque de DoS usando o script JMeter](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)

