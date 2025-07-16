---
title: Práticas recomendadas para regras de filtro de tráfego incluindo regras do WAF
description: Conheça as práticas recomendadas para configurar regras de filtro de tráfego, incluindo regras do WAF no AEM as a Cloud Service, para melhorar a segurança e reduzir riscos.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# Práticas recomendadas para regras de filtro de tráfego incluindo regras do WAF

Conheça as práticas recomendadas para configurar regras de filtro de tráfego, incluindo regras do WAF no AEM as a Cloud Service, para melhorar a segurança e reduzir riscos.

>[!IMPORTANT]
>
>As práticas recomendadas descritas neste artigo não são exaustivas e não se destinam a substituir suas próprias políticas e procedimentos de segurança.

## Práticas recomendadas gerais

- Comece com o [conjunto recomendado](./overview.md#adobe-recommended-rules) de filtros de tráfego padrão e regras de WAF fornecidas pela Adobe e ajuste-os com base nas necessidades específicas do seu aplicativo e no cenário de ameaças.
- Colabore com a equipe de segurança para determinar quais regras se alinham à postura de segurança e aos requisitos de conformidade de sua organização.
- Sempre teste regras novas ou atualizadas em ambientes de desenvolvimento antes de promovê-las para Preparo e Produção.
- Ao declarar e validar regras, comece com `action` tipo `log` para observar o comportamento sem bloquear o tráfego legítimo.
- Mova de `log` para `block` somente após analisar dados de tráfego suficientes e confirmar que nenhuma solicitação válida está sendo afetada.
- Introduzir regras de maneira incremental, envolvendo equipes de controle de qualidade, desempenho e testes de segurança para identificar efeitos colaterais não intencionais.
- Analise e analise regularmente a eficácia das regras usando as [ferramentas do painel](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). A frequência da análise (diária, semanal, mensal) deve se alinhar ao volume de tráfego e ao perfil de risco do site.
- Refine continuamente as regras com base em novas informações de ameaças, comportamento de tráfego e resultados de auditoria.

## Práticas recomendadas para regras de filtro de tráfego

- Use as [regras de filtro de tráfego padrão](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) recomendadas pela Adobe como uma linha de base, que inclui regras para borda, proteção de origem e restrições baseadas em OFAC.
- Revise alertas e registros regularmente para identificar padrões de abuso ou configuração incorreta.
- Ajuste valores limite para limites de taxa com base nos padrões de tráfego e no comportamento do usuário do seu aplicativo.

  Consulte a tabela a seguir para obter orientações sobre como escolher os valores de limite:

  | Variação | Valor |
  | :--------- | :------- |
  | Origem | Use o valor mais alto do máximo de solicitações de origem por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |
  | Borda | Use o valor mais alto do máximo de solicitações de borda por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |

  Consulte também a seção [escolhendo valores de limite](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) para obter mais detalhes.

- Mover para a ação `block` somente após a confirmação de que a ação `log` não afeta o tráfego legítimo.

## Práticas recomendadas para regras do WAF

- Comece com as [regras WAF recomendadas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) da Adobe, que incluem regras para bloquear IPs inválidos conhecidos, detectar ataques de DDoS e mitigar o abuso de bot.
- O sinalizador do WAF `ATTACK` deve alertá-lo sobre possíveis ameaças. Verifique se não há falsos positivos antes de passar para `block`.
- Se as regras recomendadas da WAF não cobrirem ameaças específicas, considere criar regras personalizadas com base nos requisitos exclusivos de seu aplicativo. Veja uma lista completa de [sinalizadores do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) na documentação.

## Regras de execução

Saiba como implementar regras de filtro de tráfego e regras do WAF no AEM as a Cloud Service:

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
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Proteção de sites da AEM usando regras do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Proteção de sites da AEM usando regras do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Proteção de sites da AEM usando regras do WAF">Protegendo sites do AEM usando regras do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando regras do WAF (Web Application Firewall) recomendadas pela Adobe no AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ativar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Recursos adicionais

- [Regras De Filtro De Tráfego, Incluindo Regras Do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Noções básicas sobre prevenção De DoS/DoS no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Bloqueando ataques DeS e DeS com regras de filtro de tráfego](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

