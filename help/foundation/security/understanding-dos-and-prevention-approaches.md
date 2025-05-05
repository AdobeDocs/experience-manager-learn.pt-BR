---
title: Noções básicas sobre prevenção de DoS/DDoS
description: Saiba como impedir e mitigar ataques de DoS e DDoS contra o AEM.
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
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# Noções básicas sobre prevenção de DoS/DDoS no AEM

Saiba mais sobre as opções disponíveis para impedir e mitigar ataques de DoS e DDoS em seu ambiente do AEM. Antes de mergulhar nos mecanismos de prevenção, forneça uma breve visão geral do [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) e do [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- Os ataques de DoS (Negação de serviço) e DDoS (Negação de serviço distribuída) são tentativas mal-intencionadas de interromper o funcionamento normal de um servidor, serviço ou rede de destino, tornando-o inacessível aos usuários desejados.
- Os ataques de DoS normalmente se originam de uma única fonte, enquanto os ataques de DDoS vêm de várias fontes.
- Os ataques de DDoS geralmente são maiores em escala em comparação aos ataques de DoS devido aos recursos combinados de vários dispositivos de ataque.
- Esses ataques são realizados inundando o target com tráfego excessivo e explorando vulnerabilidades em protocolos de rede.

A tabela a seguir descreve como impedir e mitigar ataques de DoS e DDoS:

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
            <td>Firewall de aplicativo da Web (WAF)</td>
            <td>Uma solução de segurança projetada para proteger aplicativos da Web contra vários tipos de ataques.</td>
            <td>
            <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">Licença de Proteção WAF-DDoS</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> ou <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF via contrato AMS.</td>
            <td>Seu WAF preferido</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>O ModSecurity (também conhecido como módulo Apache "mod_security") é uma solução de código aberto e entre plataformas que fornece proteção contra uma variedade de ataques contra aplicativos web.<br/> No AEM as a Cloud Service, isso só é aplicável ao serviço de Publicação do AEM, pois não há um Apache Web Server e um AEM Dispatcher na frente do serviço de Autor do AEM.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Ativar ModSecurity </a></td>
        </tr>
        <tr>
            <td>Regras de filtro de tráfego</td>
            <td>As regras de filtro de tráfego podem ser usadas para bloquear ou permitir solicitações na camada CDN.</td>
            <td><a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Exemplo de regras de filtro de tráfego</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> ou <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> regra limitando recursos.</td>
            <td>Sua solução preferida</td>
        </tr>
    </tbody>
</table>

## Análise pós-incidente e melhoria contínua

Embora não haja um fluxo padrão único para identificar e impedir ataques de DoS/DDoS e ele dependa do processo de segurança de sua organização. A **análise pós-incidente e o aprimoramento contínuo** é uma etapa crucial no processo. Estas são algumas das práticas recomendadas a serem consideradas:

- Identifique a causa raiz do ataque DoS/DDoS realizando uma análise pós-incidente, incluindo a análise de registros, tráfego de rede e configurações do sistema.
- Melhorar os mecanismos de prevenção com base nos resultados da análise pós-incidente.

