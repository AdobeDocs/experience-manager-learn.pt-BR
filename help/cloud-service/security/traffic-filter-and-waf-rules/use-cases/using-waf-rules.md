---
title: Proteção de sites da AEM usando regras do WAF
description: Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando regras do WAF (Web Application Firewall) recomendadas pela Adobe no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
badgeLicense: label="Requer uma licença" type="positive" before-title="true"
jira: KT-18308
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 7%

---

# Proteção de sites da AEM usando regras do WAF

Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando _regras recomendadas pela Adobe_ **Firewall de Aplicativo Web (WAF)** no AEM as a Cloud Service.

Os ataques sofisticados são caracterizados por altas taxas de solicitação, padrões complexos e o uso de técnicas avançadas para ignorar as medidas de segurança tradicionais.

>[!IMPORTANT]
>
> As regras de filtro de tráfego do WAF exigem uma licença adicional de **Proteção de WAF-DDoS** ou **Segurança aprimorada**. As regras de filtro de tráfego padrão estão disponíveis para clientes do Sites e do Forms por padrão.

## Objetivos de aprendizagem

- Revise as regras recomendadas pela Adobe para o WAF.
- Defina, implante, teste e analise os resultados das regras.
- Entenda quando e como refinar as regras com base nos resultados.
- Saiba como usar o Centro de ações da AEM para revisar alertas gerados pelas regras.

### Visão geral da implementação

As etapas de implementação incluem:

- Adicionar as regras do WAF ao arquivo `/config/cdn.yaml` do projeto WKND do AEM.
- Confirmar e enviar as alterações para o repositório Git do Cloud Manager.
- Implantar as alterações no ambiente do AEM usando o pipeline de configuração do Cloud Manager.
- Testando as regras simulando um ataque de DDoS usando [Nikto](https://github.com/sullo/nikto/wiki).
- Analisar os resultados usando os logs de CDN do AEM CS e a ferramenta do painel ELK.

## Pré-requisitos

Antes de continuar, verifique se você concluiu a configuração necessária, conforme descrito no [tutorial Como configurar o filtro de tráfego e as regras do WAF](../setup.md). Além disso, você clonou e implantou o [Projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) no seu ambiente AEM.

## Revisar e definir regras

As regras do WAF (Web Application Firewall) recomendadas pela Adobe são essenciais para proteger sites da AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot. Os ataques sofisticados são frequentemente caracterizados por altas taxas de solicitação, padrões complexos e o uso de técnicas avançadas (ataques baseados em protocolo ou em carga útil) para ignorar as medidas de segurança tradicionais.

Vamos analisar três regras recomendadas do WAF que devem ser adicionadas ao arquivo `cdn.yaml` no projeto WKND do AEM:

### &#x200B;1. Bloqueie ataques de IPs mal-intencionados conhecidos

Esta regra **bloqueia** solicitações que parecem suspeitas *e* originadas de endereços IP sinalizados como mal-intencionados. Como ambos os critérios são atendidos, podemos ter certeza de que o risco de falsos positivos (bloqueio do tráfego legítimo) é muito baixo. Os IPs inválidos conhecidos são identificados com base em feeds de inteligência de ameaças e outras fontes.

O sinalizador WAF `ATTACK-FROM-BAD-IP` é usado para identificar essas solicitações. Ele agrega vários sinalizadores WAF [listados aqui](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list).

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: attacks-from-bad-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: block
        wafFlags:
          - ATTACK-FROM-BAD-IP
```

### &#x200B;2. Registre (e bloqueie posteriormente) ataques de qualquer IP globalmente

Esta regra **registra** solicitações identificadas como possíveis ataques, mesmo que os endereços IP não sejam encontrados nos feeds de inteligência contra ameaças.

O sinalizador WAF `ATTACK` é usado para identificar essas solicitações. Semelhante ao `ATTACK-FROM-BAD-IP`,   agrega vários sinalizadores do WAF.

Essas solicitações provavelmente são mal-intencionadas, mas como os endereços IP não são identificados nos feeds de inteligência contra ameaças, pode ser prudente iniciar no modo `log` em vez de no modo de bloqueio. Analise os logs para falsos positivos e, uma vez validado, **mude a regra para o modo `block`**.

```yaml
...
    - name: attacks-from-any-ips-globally
      when:
        reqProperty: tier
        in: ["author", "publish"]
      action:
        type: log
        alert: true
        wafFlags:
          - ATTACK
```

Como alternativa, você pode optar por usar o modo `block` imediatamente, se os requisitos da sua empresa forem tais que você não deseja ter nenhuma chance de permitir tráfego mal-intencionado.

Essas regras recomendadas da WAF fornecem uma camada adicional de segurança contra ameaças conhecidas e emergentes.

![Regras do WKND WAF](../assets/use-cases/wknd-cdn-yaml-waf-rules.png)

## Migração para as regras mais recentes recomendadas pela Adobe para o WAF

Antes da introdução dos sinalizadores do WAF `ATTACK-FROM-BAD-IP` e `ATTACK` (em julho de 2025), as regras do WAF recomendadas eram as seguintes. Eles continham uma lista de sinalizadores WAF específicos para bloquear solicitações que correspondiam a determinados critérios, como `SANS`, `TORNODE`, `NOUA`, etc.

```yaml
...
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
...
```

A regra acima ainda é válida, mas é recomendável migrar para as novas regras que usam os sinalizadores `ATTACK-FROM-BAD-IP` e `ATTACK` do WAF _, desde que você ainda não tenha personalizado o `wafFlags` para atender às suas necessidades comerciais_.

Você pode migrar para as novas regras para manter a consistência com as práticas recomendadas seguindo estas etapas:

- Revise as regras existentes do WAF no arquivo `cdn.yaml`, que podem ser semelhantes ao exemplo acima. Confirme se não há nenhuma personalização do `wafFlags` específica para as necessidades da sua empresa.

- Substitua suas regras existentes do WAF pelas novas regras recomendadas pela Adobe WAF que usam os sinalizadores `ATTACK-FROM-BAD-IP` e `ATTACK`. Certifique-se de que todas as regras estejam no modo de bloqueio.

Se você tiver personalizado o `wafFlags` anteriormente, ainda será possível migrar para essas novas regras, mas com cuidado, garantindo que qualquer personalização seja antecipada para as regras revisadas.

A migração deve ajudá-lo a simplificar suas regras do WAF e, ao mesmo tempo, fornecer uma proteção robusta contra ameaças sofisticadas. As novas regras foram projetadas para serem mais eficazes e fáceis de gerenciar.


## Implantar regras

Para implantar as regras acima, siga estas etapas:

- Confirme e envie as alterações ao repositório do Git do Cloud Manager.

- Implante as alterações no ambiente do AEM usando o pipeline de configuração do Cloud Manager [criado anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

  ![Pipeline de configuração do Cloud Manager](../assets/use-cases/cloud-manager-config-pipeline.png)

## Testar regras

Para verificar a eficácia das regras do WAF, simule um ataque usando o [Nikto](https://github.com/sullo/nikto), um verificador de servidor Web que detecta vulnerabilidades e configurações incorretas. O comando a seguir aciona ataques de injeção de SQL no site da WKND do AEM, que é protegido pelas regras do WAF.

```shell
$./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
```

![Simulação de ataque do Nikto](../assets/use-cases/nikto-attack.png)

Para saber mais sobre a simulação de ataques, consulte a documentação [Nikto: sintonização da varredura](https://github.com/sullo/nikto/wiki/Scan-Tuning), que informa como especificar os tipos de ataque de teste a serem incluídos ou excluídos.

## Revisar alertas

Os alertas são gerados quando as regras de filtro de tráfego são acionadas. Você pode examinar esses alertas no [Centro de Ações da AEM](https://experience.adobe.com/aem/actions-center).

![Centro de Ações do WKND AEM](../assets/use-cases/wknd-aem-action-center.png)

## Analisar resultados

Para analisar os resultados das regras de filtro de tráfego, você pode usar os logs de CDN do AEM CS e a ferramenta de painel ELK. Siga as instruções da seção de configuração [Assimilação de logs CDN](../setup.md#ingest-cdn-logs) para assimilar os logs CDN na pilha ELK.

Na captura de tela a seguir, você pode ver os logs de CDN do ambiente de desenvolvimento do AEM assimilados na pilha de ELK.

![ELK de Logs CDN WKND](../assets/use-cases/wknd-cdn-logs-elk-waf.png)

No aplicativo ELK, o **Painel do WAF** deve mostrar a
Solicitações sinalizadas e valores correspondentes nas colunas de IP do cliente (cli_ip), host, url, ação (waf_action) e nome da regra (waf_match).

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged.png)

Além disso, os painéis **Distribuição de sinalizadores do WAF** e **Principais ataques** mostram detalhes adicionais.

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-2.png)

![WKND WAF Dashboard ELK](../assets/use-cases/elk-tool-dashboard-waf-flagged-top-attacks-3.png)

### Integração do Splunk

Clientes que têm o [encaminhamento do log do Splunk habilitado](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) podem criar novos painéis para analisar os padrões de tráfego.

Para criar painéis no Splunk, siga as etapas em [Painéis do Splunk para análise de log da CDN do AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

## Quando e como refinar regras

Seu objetivo é evitar o bloqueio de tráfego legítimo e, ao mesmo tempo, proteger os sites da AEM contra ameaças sofisticadas. As regras recomendadas do WAF foram projetadas para ser um ponto de partida para sua estratégia de segurança.

Para refinar as regras, considere as seguintes etapas:

- **Monitorar padrões de tráfego**: Use os logs CDN e o painel ELK para monitorar padrões de tráfego e identificar quaisquer anomalias ou picos no tráfego. Preste atenção aos painéis _Distribuição de sinalizadores do WAF_ e _Principais ataques_ no painel ELK para entender os tipos de ataques que estão sendo detectados.
- **Ajustar wafFlags**: se `ATTACK` sinalizadores estiverem sendo acionados com muita frequência ou
se você precisar ajustar o vetor de ataque, poderá criar regras personalizadas com sinalizadores WAF específicos. Veja uma lista completa de [sinalizadores do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) na documentação. Considere primeiro experimentar novas regras personalizadas no modo `log`.
- **Mover para regras de bloqueio**: depois de validar os padrões de tráfego e ajustar os sinalizadores do WAF, você pode considerar mover para regras de bloqueio.

## Resumo

Neste tutorial, você aprendeu a proteger sites da AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando regras do WAF (Web Application Firewall) recomendadas pela Adobe.

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

- [Regras de início recomendadas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules)
- [lista de sinalizadores do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list)
