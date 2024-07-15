---
title: Proteção de sites com regras de filtro de tráfego (incluindo regras WAF)
description: Saiba mais sobre as regras de Filtro de tráfego, incluindo a subcategoria de regras do Firewall de aplicações Web (WAF). Como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites de AEM.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Proteção de sites com regras de filtro de tráfego (incluindo regras WAF)

Saiba mais sobre as **regras de filtro de tráfego**, incluindo sua subcategoria de **regras do WAF (Web Application Firewall)** no AEM as a Cloud Service (AEMCS). Leia sobre como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Visão geral

Reduzir o risco de violações de segurança é a principal prioridade de qualquer organização. O AEMCS oferece o recurso de regras de filtro de tráfego, incluindo regras WAF, para proteger sites e aplicativos.

As regras de filtro de tráfego são implantadas no [CDN interno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) e são avaliadas antes que a solicitação atinja a infraestrutura do AEM. Com esse recurso, você pode melhorar significativamente a segurança do seu site, garantindo que somente solicitações legítimas tenham permissão para acessar a infraestrutura do AEM.

Este tutorial o orienta pelo processo de criação, implantação, teste e análise dos resultados das regras de filtro de tráfego, incluindo as regras WAF.

Você pode ler mais sobre as regras de filtro de tráfego em [este artigo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Uma subcategoria de regras de filtro de tráfego chamada &quot;regras WAF&quot; requer uma licença WAF-DDoS Protection ou Enhanced Security.

Convidamos você a fornecer feedback ou fazer perguntas sobre as regras do filtro de tráfego enviando um email para **aemcs-waf-adopter@adobe.com**.

## Próxima etapa

Saiba [como configurar](./how-to-setup.md) o recurso para poder criar, implantar e testar regras de filtro de tráfego. Leia sobre a configuração da ferramenta de painel de pilha **Elasticsearch, Logstash e Kibana (ELK)** para analisar os resultados dos logs de CDN do AEMCS.


