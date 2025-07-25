---
title: 'Visão geral: proteger sites do AEM'
description: Saiba como proteger os seus sites do AEM contra DoS, DDoS e tráfego malicioso com regras de filtro de tráfego, incluindo a subcategoria de regras do firewall de aplicativos web (WAF, na sigla em inglês), no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 100%

---

# Visão geral: proteger sites do AEM

Saiba como proteger os seus sites do AEM contra DoS (negação de serviço), DDoS (negação de serviço distribuída), tráfego malicioso e ataques sofisticados com **regras de filtro de tráfego**, incluindo a subcategoria de **regras do firewall de aplicativos web (WAF, na sigla em inglês)**, no AEM as a Cloud Service.

Você também aprenderá as diferenças entre o filtro de tráfego padrão e as regras de filtro de tráfego do WAF, quando devem ser usados e como começar a usar as regras recomendadas pela Adobe.

>[!IMPORTANT]
>
> As regras de filtro de tráfego do WAF exigem uma licença adicional de **Proteção do WAF contra DDoS** ou **Segurança aprimorada**. As regras de filtro de tráfego padrão estão disponíveis para clientes do Sites e do Forms por padrão.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## Introdução à segurança do tráfego no AEM as a Cloud Service

O AEM as a Cloud Service utiliza uma camada da CDN integrada para proteger e otimizar a entrega do seu site. Um dos componentes mais críticos da camada da CDN é a capacidade de definir e aplicar regras de tráfego. Essas regras funcionam como um escudo protetor para ajudar a proteger o site contra abusos, usos indevidos e ataques sem sacrificar o desempenho.

A segurança do tráfego é essencial para manter o tempo de atividade, proteger dados sensíveis e garantir uma experiência fluida para usuários legítimos. O AEM fornece duas categorias de regras de segurança:

- **Regras de filtro de tráfego padrão**
- **Regras de filtro de tráfego do firewall de aplicativos web (WAF, na sigla em inglês)**

Os conjuntos de regras ajudam os clientes a impedirem ameaças comuns e sofisticadas da web, reduzir o ruído de clientes maliciosos ou que se comportam mal, e melhorar a capacidade de observação por meio do registro de solicitações, bloqueio e detecção de padrões.

## Diferença entre as regras de filtro de tráfego padrão e do WAF

| Destaque | Regras de filtro de tráfego padrão | Regras de filtro de tráfego do WAF |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Propósito | Evite abusos como DoS, DDoS, raspagem de dados ou atividades de bot | Detectar e reagir a padrões de ataque sofisticados (por exemplo, os 10 principais da OWASP), o que também protege contra bots |
| Exemplos | Limitação de taxas, bloqueio geográfico, filtragem de agentes usuários | Injeção de SQL, XSS, IPs de ataque conhecidos |
| Flexibilidade | Altamente configurável via YAML | Altamente configurável via YAML, com sinalizadores do WAF predefinidos |
| Modo recomendado | Começar com o modo `log` e passar para o modo `block` | Comece com o modo `block` para o sinalizador do WAF `ATTACK-FROM-BAD-IP` e o modo `log` para o sinalizador do WAF `ATTACK`, e passe para o modo `block` para ambos |
| Implantação | Definido no YAML e implantado por meio do pipeline de configuração do Cloud Manager | Definido no YAML com `wafFlags` e implantado por meio do pipeline de configuração do Cloud Manager |
| Licenciamento | Incluso nas licenças do Sites e do Forms | **Requer licença de proteção do WAF contra DDoS ou segurança aprimorada** |

As regras de filtro de tráfego padrão são úteis para aplicar políticas específicas da empresa, como limites de taxas ou bloqueio de regiões específicas, bem como para bloquear o tráfego com base em propriedades de solicitações e cabeçalhos, como endereço IP, caminho ou agente usuário.
As regras de filtro de tráfego do WAF, por outro lado, fornecem uma proteção proativa abrangente para explorações da web e vetores de ataque conhecidos, contando com uma inteligência avançada para limitar falsos positivos (ou seja, bloquear o tráfego legítimo).
Para definir ambos os tipos de regra, use a sintaxe YAML; consulte [Sintaxe de regras de filtro de tráfego](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax) para mais detalhes.

## Quando e por que devem ser usadas

**Use regras de filtro de tráfego padrão** quando:

- Você quiser aplicar limites específicos da organização, como limitação de taxas de IP.
- Você estiver ciente de padrões específicos (por exemplo, endereços IP, regiões ou cabeçalhos maliciosos) que precisam de filtragem.

**Use regras de filtro de tráfego do WAF** quando:

- Você quiser uma **proteção proativa** abrangente contra padrões de ataques conhecidos e disseminados (por exemplo, injeção, abuso de protocolo), bem como IPs maliciosos conhecidos, coletados de fontes de dados especializadas.
- Você quiser recusar solicitações maliciosas e, ao mesmo tempo, limitar a chance de bloquear o tráfego legítimo.
- Você quiser limitar a atividade de defesa contra ameaças comuns e sofisticadas, aplicando regras de configuração simples.

Juntas, essas regras fornecem uma estratégia de defesa detalhada que permite que os clientes do AEM as a Cloud Service tomem medidas proativas e reativas na proteção de suas propriedades digitais.

## Regras recomendadas pela Adobe

A Adobe fornece regras de filtro de tráfego padrão e de filtro de tráfego do WAF recomendadas para ajudar a proteger rapidamente os seus sites do AEM.

- **Regras de filtro de tráfego padrão** (disponíveis por padrão): aborde casos comuns de abuso, como DoS, DDoS e ataques de bot contra **a borda da CDN**, **a origem** ou o tráfego de países sancionados.\
  Por exemplo:
   - Limitação de taxas de IPs que fazem mais de 500 solicitações/segundo _na borda da CDN_
   - Limitação de taxas de IPs que fazem mais de 100 solicitações/segundo _na origem_
   - Bloqueio do tráfego de países listados pelo Gabinete de Controle de Ativos Estrangeiros (OFAC, na sigla em inglês)

- **Regras de filtro de tráfego do WAF** (exige uma licença complementar): fornece uma proteção adicional contra ameaças sofisticadas, incluindo [os 10 principais da OWASP](https://owasp.org/www-project-top-ten/), como injeção de SQL, criação de script entre sites (XSS) e outros ataques a aplicativos web.
Por exemplo:
   - Bloquear solicitações de endereços IP maliciosos conhecidos
   - Registrar ou bloquear solicitações suspeitas sinalizadas como ataques

>[!TIP]
>
> Para começar, aplique as **regras recomendadas pela Adobe** para beneficiar-se da experiência em segurança e das atualizações contínuas da Adobe. Se a sua empresa estiver enfrentando riscos específicos ou casos na borda, ou se notar falsos positivos (bloqueio do tráfego legítimo), você poderá definir **regras personalizadas** ou estender o conjunto padrão de acordo com as suas necessidades.

## Introdução

Saiba como definir, implantar, testar e analisar regras de filtro de tráfego, incluindo regras do WAF, no AEM as a Cloud Service, seguindo o guia de configuração e os casos de uso abaixo. Isso fornece o conhecimento básico necessário para aplicar com confiança as regras recomendadas pela Adobe posteriormente.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Como configurar regras de filtro de tráfego, incluindo regras do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="Como configurar regras de filtro de tráfego, incluindo regras do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Como configurar regras de filtro de tráfego, incluindo regras do WAF">Como configurar regras de filtro de tráfego, incluindo regras do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como configurar para criar, implantar, testar e analisar os resultados das regras de filtro de tráfego, incluindo regras do WAF.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Começar agora</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Guia de configuração de regras recomendadas pela Adobe

Este guia fornece instruções passo a passo para configurar e implantar as regras de filtro de tráfego padrão e de filtro de tráfego do WAF recomendadas pela Adobe no seu ambiente do AEM as a Cloud Service.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
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
                    <p class="is-size-6">Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot, usando regras de filtro de tráfego do firewall de aplicativos web (WAF, na sigla em inglês) recomendadas pela Adobe no AEM as a Cloud Service.</p>
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

Para casos mais avançados, você pode explorar os seguintes casos de uso, que demonstram como implementar regras de filtro de tráfego personalizadas com base em requisitos específicos da empresa:

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
                    <a href="./how-to/request-logging.md" title="Monitorar solicitações sensíveis" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Monitorar solicitações sensíveis"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Monitorar solicitações sensíveis">Monitorar solicitações sensíveis</a>
                    </p>
                    <p class="is-size-6">Saiba como monitorar solicitações sensíveis, registrando-as com regras de filtro de tráfego no AEM as a Cloud Service.</p>
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
                    <a href="./how-to/request-blocking.md" title="Restringir o acesso" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="Restringir o acesso"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="Restringir o acesso">Restringir o acesso</a>
                    </p>
                    <p class="is-size-6">Saiba como restringir o acesso, bloqueando solicitações específicas com regras de filtro de tráfego no AEM as a Cloud Service.</p>
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
                    <a href="./how-to/request-transformation.md" title="Normalizar solicitações" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="Normalizar solicitações"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Normalizar solicitações">Normalizar solicitações</a>
                    </p>
                    <p class="is-size-6">Saiba como normalizar solicitações, transformando-as com regras de filtro de tráfego no AEM as a Cloud Service.</p>
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

- [Regras de filtro de tráfego, incluindo regras do WAF](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
