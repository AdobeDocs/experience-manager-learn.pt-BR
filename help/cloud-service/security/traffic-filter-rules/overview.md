---
title: Proteção de sites com regras de filtro de tráfego (incluindo regras do WAF)
description: Saiba mais sobre as regras de Filtro de tráfego, incluindo a subcategoria de regras do Firewall de aplicativos Web (WAF). Como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites do AEM.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Proteção de sites com regras de filtro de tráfego (incluindo regras do WAF)

Saiba mais sobre as **regras de filtro de tráfego**, incluindo sua subcategoria de **regras do Firewall do Aplicativo Web (WAF)** no AEM as a Cloud Service (AEMCS). Leia sobre como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Visão geral

Reduzir o risco de violações de segurança é a principal prioridade de qualquer organização. O AEM CS oferece o recurso de regras de filtro de tráfego, incluindo regras do WAF, para proteger sites e aplicativos.

As regras de filtro de tráfego são implantadas no [CDN interno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=pt-BR) e são avaliadas antes que a solicitação atinja a infraestrutura da AEM. Com esse recurso, você pode melhorar significativamente a segurança do seu site, garantindo que somente solicitações legítimas tenham permissão para acessar a infraestrutura do AEM.

Este tutorial o orienta pelo processo de criação, implantação, teste e análise dos resultados das regras de filtro de tráfego, incluindo as regras do WAF.

Você pode ler mais sobre as regras de filtro de tráfego em [este artigo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=pt-BR).

>[!IMPORTANT]
>
> Uma subcategoria de regras de filtro de tráfego chamada de &quot;regras do WAF&quot; requer uma licença de Proteção WAF-DDoS ou Segurança aprimorada.

Convidamos você a fornecer feedback ou fazer perguntas sobre as regras do filtro de tráfego enviando um email para **aemcs-waf-adopter@adobe.com**.

## Próxima etapa

Saiba [como configurar](./how-to-setup.md) o recurso para poder criar, implantar e testar regras de filtro de tráfego. Leia sobre a configuração da ferramenta de painel de pilha **Elasticsearch, Logstash e Kibana (ELK)** para analisar os resultados dos logs de CDN do AEMCS.


