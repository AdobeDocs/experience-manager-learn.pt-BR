---
title: Proteger sites com regras de filtro de tráfego (incluindo regras do WAF)
description: Saiba mais sobre as regras de filtro de tráfego, incluindo a subcategoria de regras do firewall de aplicativos da web (WAF). Como criar, implantar e testar as regras. Além disso, analise os resultados para proteger os seus sites do AEM.
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
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Proteger sites com regras de filtro de tráfego (incluindo regras do WAF)

Saiba mais sobre as **regras de filtro de tráfego**, incluindo a subcategoria de **regras do firewall de aplicativos da web (WAF)** no AEM as a Cloud Service (AEMCS). Saiba mais sobre como criar, implantar e testar as regras. Além disso, analise os resultados para proteger os seus sites do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Visão geral

Reduzir o risco de violações de segurança é a prioridade de qualquer organização. O AEMCS oferece o recurso de regras de filtro de tráfego, incluindo regras do WAF, para proteger sites e aplicativos.

As regras de filtro de tráfego são implantadas na [CDN integrada](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) e avaliadas antes que a solicitação atinja a infraestrutura do AEM. Com esse recurso, você pode aumentar significativamente a segurança do seu site, garantindo que somente solicitações legítimas tenham permissão para acessar a infraestrutura do AEM.

Este tutorial apresenta o processo de criação, implantação, teste e análise dos resultados das regras de filtro de tráfego, incluindo regras do WAF.

Você pode ler mais sobre as regras de filtro de tráfego [neste artigo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf).

>[!IMPORTANT]
>
> Uma subcategoria de regras de filtro de tráfego denominada “regras do WAF” requer uma licença de proteção WAF-DDoS ou de segurança aprimorada.

Você pode dar o seu feedback ou fazer perguntas sobre as regras de filtro de tráfego, enviando um email para **aemcs-waf-adopter@adobe.com**.

## Próxima etapa

Saiba [como configurar](./how-to-setup.md) o recurso para poder criar, implantar e testar regras de filtro de tráfego. Saiba mais sobre a configuração das ferramentas do painel de pilhas **Elasticsearch, Logstash e Kibana (ELK)** para analisar os resultados dos logs da CDN do AEMCS.


