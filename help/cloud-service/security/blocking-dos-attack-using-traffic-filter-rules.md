---
title: Bloqueio de DoS, DDoS e ataques sofisticados usando regras de filtro de tráfego
description: Saiba como bloquear DoS, DDoS e ataques sofisticados usando regras de filtro de tráfego no AEM as a Cloud Service.
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
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 32%

---

# Bloqueio de DoS, DDoS e ataques sofisticados usando regras de filtro de tráfego

Saiba como bloquear Negação de Serviço (DoS), Negação de Serviço Distribuída (DDoS) e ataques sofisticados usando **regras de filtro de tráfego** na CDN gerenciada pelo AEM as a Cloud Service (AEMCS).

Esses ataques causam picos de tráfego na CDN e possivelmente no serviço de publicação do AEM (na origem) e podem afetar a capacidade de resposta e a disponibilidade dos sites.

Este artigo fornece uma visão geral das proteções padrão para o site da AEM e como estendê-las por meio da configuração do cliente. Ele também descreve como analisar padrões de tráfego e configurar regras padrão de filtro de tráfego para bloquear esses ataques.

## Proteções padrão no AEM as a Cloud Service

Vamos entender as proteções contra DDoS padrão para o seu site do AEM:

- **Armazenamento em cache:** com boas políticas de armazenamento em cache, o impacto de um ataque de DDoS é mais limitado, porque a CDN impede que a maioria das solicitações atinja a origem e cause uma degradação do desempenho.
- **Dimensionamento automático:** os serviços de criação e publicação do AEM são dimensionados automaticamente para lidar com picos de tráfego, embora ainda possam ser afetados por grandes e súbitos aumentos de tráfego.
- **Bloqueio:** a CDN da Adobe bloqueará o tráfego para a origem se ele exceder uma taxa definida pela Adobe a partir de um endereço IP específico por PoP (ponto de presença) da CDN.
- **Alertas:** o centro de ações envia uma notificação de alerta de pico de tráfego na origem quando o tráfego excede uma taxa determinada. Esse alerta é acionado quando o tráfego para qualquer PoP da CDN excede uma taxa de solicitações _definida pela Adobe_ por endereço IP. Consulte [Alertas das regras de filtro de tráfego](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para obter mais detalhes.

Essas proteções integradas devem ser consideradas como uma linha de base da capacidade de uma organização de minimizar o impacto de um ataque de DDoS no desempenho. Como cada site tem características de desempenho diferentes e pode observar que a degradação do desempenho antes do limite de taxa definido pela Adobe seja atendida, é recomendável estender as proteções padrão por meio da _configuração do cliente_.

## Extensão da proteção com regras de filtro de tráfego

Vamos analisar algumas medidas adicionais e recomendadas que os clientes podem tomar para proteger seus sites contra ataques de DDoS:

- Implemente as [regras padrão de filtro de tráfego](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md) recomendadas pela Adobe para identificar padrões de tráfego potencialmente maliciosos registrando e alertando sobre comportamentos suspeitos.
- Use o complemento **Proteção WAF-DDoS** ou **Segurança aprimorada** e implemente as [Regras de Filtro de Tráfego do WAF](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md) recomendadas pela Adobe para defender-se contra ataques sofisticados, incluindo aqueles que usam protocolo avançado ou técnicas baseadas em carga.
- Aumente a cobertura do cache configurando [solicitar transformações](./traffic-filter-and-waf-rules/how-to/request-transformation.md) para ignorar parâmetros de consulta desnecessários.

## Introdução

Explore os seguintes tutoriais para configurar as regras recomendadas pela Adobe para bloquear ataques.

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="Como configurar regras de filtro de tráfego, incluindo regras do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="Como configurar regras de filtro de tráfego, incluindo regras do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="Como configurar regras de filtro de tráfego, incluindo regras do WAF">Como configurar regras de filtro de tráfego, incluindo regras do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como configurar o para criar, implantar, testar e analisar os resultados das regras de filtro de tráfego, incluindo regras do WAF.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Iniciar Agora</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="Proteção de sites da AEM usando regras padrão de filtro de tráfego" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="Proteção de sites da AEM usando regras padrão de filtro de tráfego"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Proteção de sites da AEM usando regras padrão de filtro de tráfego">Protegendo sites da AEM usando regras de filtro de tráfego padrão</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra DoS, DDoS e abuso de bot usando regras de filtro de tráfego padrão recomendadas pela Adobe no AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aplicar regras</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="Proteção de sites da AEM usando regras de filtro de tráfego do WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="Proteção de sites da AEM usando regras de filtro de tráfego do WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Proteção de sites da AEM usando regras de filtro de tráfego do WAF">Protegendo sites da AEM usando as regras de filtro de tráfego do WAF</a>
                    </p>
                    <p class="is-size-6">Saiba como proteger sites do AEM contra ameaças sofisticadas, incluindo DoS, DDoS e abuso de bot usando regras de filtro de tráfego do Firewall de aplicativo web (WAF) recomendadas pela Adobe no AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ativar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
