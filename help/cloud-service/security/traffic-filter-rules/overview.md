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
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---


# Proteção de sites com regras de filtro de tráfego (incluindo regras WAF)

Saiba mais sobre **regras de filtro de tráfego**, incluindo a sua subcategoria de **Regras do WAF (Web Application Firewall)** no AEM as a Cloud Service (AEMCS). Leia sobre como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites de AEM.

## Visão geral

Reduzir o risco de violações de segurança é a principal prioridade de qualquer organização. O AEMCS oferece o recurso de regras de filtro de tráfego, incluindo regras WAF, para proteger sites e aplicativos.

As regras de filtro de tráfego são implantadas no [CDN integrada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) e são avaliados antes que a solicitação chegue à infraestrutura do AEM. Com esse recurso, você pode melhorar significativamente a segurança do seu site, garantindo que somente solicitações legítimas tenham permissão para acessar a infraestrutura do AEM.

Este tutorial o orienta pelo processo de criação, implantação, teste e análise dos resultados das regras de filtro de tráfego, incluindo as regras WAF.

Você pode ler mais sobre as regras de filtro de tráfego em [este artigo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)

>[!IMPORTANT]
>
> Uma subcategoria de regras de filtro de tráfego chamada &quot;Regras WAF&quot; requer uma licença WAF-DDoS Protection


## Próxima etapa

Saiba mais [como configurar](./how-to-setup.md) Use o recurso para criar, implantar e testar regras de filtro de tráfego. Leia sobre como configurar o **Elasticsearch, Logstash e Kibana (ELK)** empilhe as ferramentas do painel para analisar os resultados dos logs de CDN do AEM CS.



