---
title: Visão geral - Proteção de sites da AEM
description: Saiba como proteger seus sites do AEM contra DoS, DDoS e tráfego mal-intencionado usando regras de filtro de tráfego, incluindo a subcategoria de regras do Firewall de aplicativo web (WAF) no AEM as a Cloud Service.
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
ht-degree: 1%

---

# Visão geral - Proteção de sites da AEM

Saiba como proteger seus sites da AEM contra DoS (Negação de Serviço), DDoS (Negação de Serviço Distribuída), tráfego mal-intencionado e ataques sofisticados usando **regras de filtro de tráfego**, incluindo a subcategoria de **regras do WAF (Firewall de Aplicativo Web)** no AEM as a Cloud Service.

Você também aprenderá sobre as diferenças entre o filtro de tráfego padrão e as regras de filtro de tráfego do WAF, quando usá-los e como começar a usar as regras recomendadas pela Adobe.

>[!IMPORTANT]
>
> As regras de filtro de tráfego do WAF exigem uma licença adicional de **Proteção de WAF-DDoS** ou **Segurança aprimorada**. As regras de filtro de tráfego padrão estão disponíveis para clientes do Sites e do Forms por padrão.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## Introdução à segurança de tráfego no AEM as a Cloud Service

O AEM as a Cloud Service aproveita uma camada de CDN integrada para proteger e otimizar a entrega do seu site. Um dos componentes mais críticos da camada de CDN é a capacidade de definir e aplicar regras de tráfego. Essas regras funcionam como uma blindagem protetora para ajudar a proteger o site contra abuso, uso indevido e ataques — sem sacrificar o desempenho.

A segurança do tráfego é essencial para manter o tempo de atividade, proteger dados confidenciais e garantir uma experiência perfeita para usuários legítimos. O AEM fornece duas categorias de regras de segurança:

- **Regras padrão de filtro de tráfego**
- **Regras de filtro de tráfego do WAF (Firewall do Aplicativo Web)**

Os conjuntos de regras ajudam os clientes a impedir ameaças comuns e sofisticadas da Web, reduzir o ruído de clientes mal-intencionados ou que se comportam mal e melhorar a capacidade de observação por meio de registro de solicitações, bloqueio e detecção de padrões.

## Diferença entre as regras de filtro de tráfego padrão e do WAF

| Destaque | Regras padrão de filtro de tráfego | Regras de filtro de tráfego do WAF |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Propósito | Evite abusos como DoS, DDoS, raspagem ou atividade de bot | Detectar e reagir a padrões de ataque sofisticados (por exemplo, OWASP Top 10), que também protege de bots |
| Exemplos | Limitação de taxa, bloqueio geográfico, filtragem agente-usuário | Injeção de SQL, XSS, IPs de ataque conhecidos |
| Flexibilidade | Altamente configurável via YAML | Altamente configurável via YAML, com sinalizadores predefinidos do WAF |
| Modo recomendado | Iniciar com o modo `log` e mover para o modo `block` | Comece com o modo `block` para o Sinalizador do WAF `ATTACK-FROM-BAD-IP` e o modo `log` para o Sinalizador do WAF `ATTACK` e mova para o modo `block` para ambos |
| Implantação | Definido no YAML e implantado por meio do pipeline de configuração do Cloud Manager | Definido no YAML com `wafFlags` e implantado via Pipeline de configuração do Cloud Manager |
| Licenciamento | Incluído nas licenças do Sites e do Forms | **Requer proteção WAF-DDoS ou licença de Segurança aprimorada** |

As regras padrão de filtro de tráfego são úteis para aplicar políticas específicas da empresa, como limites de taxa ou bloqueio de regiões específicas, bem como para bloquear o tráfego com base em propriedades de solicitação e cabeçalhos, como endereço IP, caminho ou agente do usuário.
As regras de filtro de tráfego do WAF, por outro lado, fornecem proteção proativa abrangente para explorações da Web e vetores de ataque conhecidos e têm inteligência avançada para limitar falsos positivos (ou seja, bloquear o tráfego legítimo).
Para definir ambos os tipos de regras, use a sintaxe YAML, consulte [Sintaxe de regras de filtro de tráfego](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax) para obter mais detalhes.

## Quando e por que usá-los

**Usar regras de filtro de tráfego padrão** quando:

- Você deseja aplicar limites específicos da organização, como limitação de taxa de IP.
- Você está ciente de padrões específicos (por exemplo, endereços IP mal-intencionados, regiões, cabeçalhos) que precisam de filtragem.

**Usar as regras de filtro de tráfego do WAF** quando:

- Você deseja uma **proteção pró-ativa** abrangente contra padrões de ataques conhecidos e disseminados (por exemplo, injeção, abuso de protocolo), bem como IPs mal-intencionados conhecidos, coletados de fontes de dados especializadas.
- Você deseja negar solicitações mal-intencionadas e, ao mesmo tempo, limitar a chance de bloquear tráfego legítimo.
- Você deseja limitar o esforço de defesa contra ameaças comuns e sofisticadas, aplicando regras de configuração simples.

Juntas, essas regras fornecem uma estratégia de defesa detalhada que permite aos clientes da AEM as a Cloud Service tomar medidas proativas e reativas na proteção de suas propriedades digitais.

## Regras recomendadas pela Adobe

O Adobe fornece regras recomendadas para o filtro de tráfego padrão e regras de filtro de tráfego do WAF para ajudar você a proteger rapidamente seus sites do AEM.

- **Regras padrão de filtro de tráfego** (disponíveis por padrão): aborde cenários comuns de abuso, como DoS, DDoS e ataques de bot contra **borda da CDN**, **origem** ou tráfego de países sancionados.\
  Os exemplos incluem:
   - IPs de limitação de taxa que fazem mais de 500 solicitações/segundo _na borda da CDN_
   - IPs de limitação de taxa que fazem mais de 100 solicitações/segundo _na origem_
   - Bloqueando o tráfego de países listados pelo Office of Foreign Assets Control (OFAC)

- **Regras de filtro de tráfego do WAF** (requer licença complementar): fornece proteção adicional contra ameaças sofisticadas, incluindo [OWASP Top Ten](https://owasp.org/www-project-top-ten/) ameaças como injeção de SQL, script entre sites (XSS) e outros ataques de aplicativos Web.
Os exemplos incluem:
   - Bloquear solicitações de endereços IP inválidos conhecidos
   - Registro ou bloqueio de solicitações suspeitas sinalizadas como ataques

>[!TIP]
>
> Comece aplicando as **regras recomendadas pela Adobe** para se beneficiar da experiência em segurança e das atualizações contínuas da Adobe. Se a sua empresa tiver riscos específicos, casos de borda ou notar falsos positivos (bloqueio de tráfego legítimo), você poderá definir **regras personalizadas** ou estender o conjunto padrão para atender às suas necessidades.

## Introdução

Saiba como definir, implantar, testar e analisar regras de filtro de tráfego, incluindo regras do WAF, no AEM as a Cloud Service seguindo o guia de configuração e os casos de uso abaixo. Isso fornece o conhecimento de fundo para que você possa aplicar com confiança as regras recomendadas pela Adobe posteriormente.

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
                    <p class="is-size-6">Saiba como configurar o para criar, implantar, testar e analisar os resultados das regras de filtro de tráfego, incluindo regras do WAF.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Iniciar Agora</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## guia de configuração de regras recomendado pela Adobe

Este guia fornece instruções passo a passo para configurar e implantar o filtro de tráfego padrão recomendado pela Adobe e as regras de filtro de tráfego do WAF no seu ambiente do AEM as a Cloud Service.

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

Para cenários mais avançados, você pode explorar os seguintes casos de uso que demonstram como implementar regras de filtro de tráfego personalizadas com base em requisitos de negócios específicos:

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
