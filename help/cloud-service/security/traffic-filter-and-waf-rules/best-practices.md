---
title: Práticas recomendadas para regras de filtro de tráfego, incluindo regras do WAF
description: Conheça as práticas recomendadas para configurar regras de filtro de tráfego, incluindo regras do WAF, no AEM as a Cloud Service, a fim de melhorar a segurança e reduzir os riscos.
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
workflow-type: ht
source-wordcount: '638'
ht-degree: 100%

---

# Práticas recomendadas para regras de filtro de tráfego, incluindo regras do WAF

Conheça as práticas recomendadas para configurar regras de filtro de tráfego, incluindo regras do WAF, no AEM as a Cloud Service, a fim de melhorar a segurança e reduzir os riscos.

>[!IMPORTANT]
>
>As práticas recomendadas descritas neste artigo não estão completas nem servem como substitutas para as suas próprias políticas e procedimentos de segurança.

## Práticas recomendadas gerais

- Comece com o [conjunto recomendado](./overview.md#adobe-recommended-rules) de filtros de tráfego padrão e regras do WAF fornecido pela Adobe, e ajuste-os com base nas necessidades específicas do seu aplicativo e no cenário de ameaças.
- Colabore com a sua equipe de segurança para determinar quais regras se alinham à postura de segurança e aos requisitos de conformidade da sua organização.
- Sempre teste regras novas ou atualizadas em ambientes de desenvolvimento antes de promovê-las para as etapas de preparo e produção.
- Ao declarar e validar regras, comece com o tipo `action` `log` para observar o comportamento sem bloquear o tráfego legítimo.
- Passe de `log` para `block` somente após analisar dados de tráfego suficientes e confirmar que nenhuma solicitação válida está sendo afetada.
- Introduza as regras de forma incremental, envolvendo as equipes de controle de qualidade, desempenho e testes de segurança para identificar efeitos colaterais indesejados.
- Revise e analise periodicamente a eficácia das regras com as [ferramentas do painel](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). A frequência da revisão (diária, semanal, mensal) deve alinhar-se ao volume de tráfego e ao perfil de risco do site.
- Refine continuamente as regras com base em novas informações sobre ameaças, comportamento do tráfego e resultados de auditorias.

## Práticas recomendadas para regras de filtro de tráfego

- Use as [regras de filtro de tráfego padrão](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) recomendadas pela Adobe como uma linha de base que inclui regras de proteção na borda e na origem, e restrições baseadas no OFAC.
- Revise alertas e logs periodicamente para identificar padrões de abuso ou configuração incorreta.
- Ajuste os valores dos limite de taxa com base nos padrões de tráfego e no comportamento dos usuários do seu aplicativo.

  Consulte a tabela a seguir para obter orientações sobre como escolher os valores de limite:

  | Variação | Valor |
  | :--------- | :------- |
  | Origem | Use o valor mais alto do máximo de solicitações de origem por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |
  | Borda | Use o valor mais alto do máximo de solicitações de borda por IP/POP em condições de tráfego **normais** (ou seja, não a taxa no momento de um DDoS) e multiplique-o |

  Confira também a seção [Escolher os valores dos limites](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) para mais detalhes.

- Passe para a ação `block` somente após confirmar que a ação `log` não afeta o tráfego legítimo.

## Práticas recomendadas para regras do WAF

- Comece com as [regras do WAF recomendadas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) pela Adobe, que incluem regras para bloquear IPs inválidos conhecidos, detectar ataques de DDoS e mitigar o abuso de bot.
- O sinalizador do WAF `ATTACK` deve alertar sobre possíveis ameaças. Verifique se não há falsos positivos antes de passar para `block`.
- Se as regras do WAF recomendadas não abrangerem ameaças específicas, considere criar regras personalizadas com base nos requisitos exclusivos do seu aplicativo. Confira uma lista completa de [Sinalizadores do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) na documentação.

## Implementar regras

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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Proteger sites do AEM com regras de filtro de tráfego padrão" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Proteger sites do AEM com regras de filtro de tráfego padrão"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Proteger sites do AEM com regras de filtro de tráfego padrão">Proteger sites do AEM com regras de filtro de tráfego padrão</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra DoS, DDoS e abuso de bot, usando as regras de filtro de tráfego padrão recomendadas pela Adobe no AEM as a Cloud Service.</p>
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
                    <a href="./use-cases/using-waf-rules.md" title="Proteger sites do AEM com regras do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Proteger sites do AEM com regras do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Proteger sites do AEM com regras do WAF">Proteger sites do AEM com regras do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot, usando regras do firewall de aplicativos web (WAF, na sigla em inglês) recomendadas pela Adobe no AEM as a Cloud Service.</p>
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

- [Regras de filtro de tráfego, incluindo regras do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Entenda a prevenção de DoS/DDoS no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Bloquear ataques de DoS e DDoS por meio de regras de filtro de tráfego](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

