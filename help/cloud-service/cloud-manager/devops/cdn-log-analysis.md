---
title: Ferramentas de análise de log da CDN
description: Saiba mais sobre a ferramenta de análise de logs de CDN do AEM Cloud Service fornecida pela Adobe e como ela ajuda a obter insights sobre o desempenho da CDN e a implementação do AEM.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Ferramentas de análise de log da CDN

Saiba mais sobre a _Ferramenta de Análise de Log da CDN do AEM Cloud Service_ fornecida pela Adobe e como ela ajuda a obter insights sobre o desempenho da CDN e a implementação do AEM.
 
>[!VIDEO](https://video.tv.adobe.com/v/3446110?quality=12&learn=on&captions=por_br)

## Visão geral

A [Ferramenta de Análise de Log da CDN do AEM as a Cloud Service](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) oferece painéis pré-criados que você pode integrar com o [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) ou a [pilha de ELK](https://www.elastic.co/elastic-stack) para monitoramento e análise em tempo real dos logs de CDN.

Com essa ferramenta, você pode obter monitoramento em tempo real e detecção proativa de problemas. Dessa forma, garantir uma entrega de conteúdo otimizada e medidas de segurança apropriadas contra ataques de DoS (Negação de Serviço) e DDoS (Negação de Serviço Distribuída).

## Principais recursos

- Análise simplificada de registros
- Monitoramento em tempo real
- Integração contínua
- Painéis para
   - Identificar possíveis ameaças à segurança
   - Experiência mais rápida do usuário final

## Visão geral do painel

Para iniciar rapidamente a análise de registro, o Adobe fornece painéis pré-criados para pilhas Splunk e ELK.

- **Taxa de Acertos do Cache do CDN**: fornece informações sobre a taxa de acertos do cache total e a contagem total de solicitações por status HIT, PASS e MISS. Ele também fornece os principais URLs de HIT, PASS e MISS.

  ![Taxa de acertos do cache do CDN](assets/CHR-dashboard.png)

- **Painel de Tráfego da CDN**: fornece informações sobre o tráfego por meio da taxa de solicitação de CDN e Origem, taxas de erro 4xx e 5xx e solicitações não armazenadas em cache. Ele também fornece o máximo de solicitações CND e Origin por segundo por endereço IP de cliente e mais insights para otimizar as configurações de CDN.

  ![Painel de Tráfego da CDN](assets/Traffic-dashboard.png)

- **Painel do WAF**: fornece informações por meio de solicitações analisadas, sinalizadas e bloqueadas. Ele também fornece os principais ataques pela WAF Flag ID, os 100 principais invasores por IP de cliente, país e agente de usuário e mais insights para otimizar as configurações do WAF.

  ![Painel do WAF](assets/WAF-Dashboard.png)

## Integração do Splunk

Para organizações que usam o [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) e que habilitaram o encaminhamento de logs do AEMCS para suas instâncias do Splunk, é possível importar rapidamente os painéis pré-criados. Essa configuração facilita a análise de registro acelerada, fornecendo insights acionáveis para otimizar as implementações do AEM e mitigar as ameaças à segurança, como ataques de DOS.

Você pode começar a usar os [painéis do Splunk para o guia de Análise de Log da CDN do AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).


## Integração de ELK

A [pilha de ELK](https://www.elastic.co/elastic-stack), composta por Elasticsearch, Logstash e Kibana, é outra opção poderosa para análise de log. É útil para organizações que não têm acesso a uma configuração do Splunk ou recursos de encaminhamento de logs. A configuração da pilha ELK localmente é simples, a ferramenta fornece o arquivo Docker Compose para começar rapidamente. Em seguida, você pode importar os painéis pré-criados e assimilar os logs de CDN baixados usando o Adobe Cloud Manager.

Você pode começar a usar o [guia de Análise de Log do ELK Docker para AEM CS CDN](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis).
