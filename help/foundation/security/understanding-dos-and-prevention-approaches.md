---
title: Entenda a prevenção de DoS/DDoS
description: Saiba como prevenir e mitigar ataques de DoS e DDoS contra o AEM.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '370'
ht-degree: 100%

---

# Entenda a prevenção de DoS/DDoS no AEM

Saiba mais sobre as opções disponíveis para prevenir e mitigar ataques de DoS e DDoS no seu ambiente do AEM. Antes de mergulhar fundo nos mecanismos de prevenção, confira uma breve visão geral do [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) e do [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- Os ataques de DoS (negação de serviço) e DDoS (negação de serviço distribuída) são tentativas mal-intencionadas de interromper o funcionamento normal de um servidor, serviço ou rede de destino, tornando-os inacessíveis para os usuários pretendidos.
- Os ataques de DoS normalmente se originam de uma única fonte, enquanto os ataques de DDoS vêm de várias fontes.
- Os ataques de DDoS geralmente são maiores em escala em comparação com os ataques de DoS devido aos recursos combinados de vários dispositivos de ataque.
- Esses ataques inundam o alvo com tráfego excessivo e exploram vulnerabilidades em protocolos de rede.

A tabela a seguir descreve como prevenir e mitigar ataques de DoS e DDoS:

<table>
    <tbody>
        <tr>
            <td><strong>Mecanismo de prevenção</strong></td>
            <td><strong>Descrição</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (no local)</strong></td>
        </tr>
        <tr>
            <td>Web Application Firewall (WAF)</td>
            <td>Uma solução de segurança projetada para proteger aplicativos da web contra vários tipos de ataque.</td>
            <td>
            <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">Licença de proteção WAF-DDoS</a></td>
            <td>WAF do <a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> ou <a href="https://azure.microsoft.com/pt-br/products/web-application-firewall" target="_blank">Azure</a> via contrato do AMS.</td>
            <td>Seu WAF preferido</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>O ModSecurity (também conhecido como módulo Apache “mod_security”) é uma solução de código aberto e multiplataforma que fornece proteção contra uma variedade de ataques a aplicativos da web.<br/> No AEM as a Cloud Service, isso só se aplica ao serviço do AEM Publish, pois não há um Apache Web Server nem um AEM Dispatcher à frente do serviço do AEM Author.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Habilitar ModSecurity </a></td>
        </tr>
        <tr>
            <td>Regras de filtro de tráfego</td>
            <td>As regras de filtro de tráfego podem ser usadas para bloquear ou permitir solicitações na camada da CDN.</td>
            <td><a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Exemplos de regras de filtro de tráfego</a></td>
            <td>Recursos de limitação de regras do <a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> ou <a href="https://learn.microsoft.com/pt-br/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>.</td>
            <td>Sua solução preferida</td>
        </tr>
    </tbody>
</table>

## Análise pós-ocorrência e aprimoramento contínuo

Não existe um fluxo padrão unificado para identificar e prevenir ataques de DoS/DDoS. Isso vai depender do processo de segurança da sua organização. A **análise pós-ocorrência e o aprimoramento contínuo** é uma etapa crucial do processo. Estas são algumas das práticas recomendadas a serem consideradas:

- Identifique a causa do ataque de DoS/DDoS, realizando uma análise pós-ocorrência, incluindo análise de logs, tráfego de rede e configurações do sistema.
- Aprimore os mecanismos de prevenção com base nos resultados da análise pós-ocorrência.

