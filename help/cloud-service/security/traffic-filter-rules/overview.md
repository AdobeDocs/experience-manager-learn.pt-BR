---
title: Proteção de sites com regras de filtro de tráfego (incluindo regras WAF)
description: Saiba mais sobre as regras de Filtro de tráfego, incluindo a subcategoria de regras do Firewall de aplicações Web (WAF). Como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites de AEM.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: bca52c7543b35fc20a782dfd3f2b2dc81bee4cde
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---


# Proteção de sites com regras de filtro de tráfego (incluindo regras WAF)

Saiba mais sobre **Regras de Filtro de tráfego**, incluindo a sua subcategoria de **Regras do WAF (Web Application Firewall)** no AEM as a Cloud Service (AEMCS). Leia sobre como criar, implantar e testar as regras. Além disso, analise os resultados para proteger seus sites de AEM.

## Visão geral

Reduzir o risco de violações de segurança é a principal prioridade de qualquer organização. O AEMCS oferece o recurso de regras de Filtro de tráfego, incluindo regras WAF, para proteger sites e aplicativos.

As regras de Filtro de tráfego são implantadas na [CDN integrada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) e são avaliados antes que a solicitação chegue à infraestrutura do AEM. Com esse recurso, você pode melhorar significativamente a segurança do seu site, garantindo que somente solicitações legítimas tenham permissão para acessar a infraestrutura do AEM.

Este tutorial o orienta pelo processo de criação, implantação, teste e análise dos resultados das regras de Filtro de tráfego, incluindo as regras WAF.

>[!IMPORTANT]
>
> A subcategoria de regras de Filtro de Tráfego chamada &quot;regras WAF&quot; requer uma licença WAF-DDoS Protection


## Próxima etapa

Saiba mais [como configurar](./how-to-setup.md) Use o recurso para criar, implantar e testar as regras de Filtro de tráfego. Leia sobre como configurar o **Elasticsearch, Logstash e Kibana (ELK)** empilhe as ferramentas do painel para analisar os resultados dos logs de CDN do AEM CS.



