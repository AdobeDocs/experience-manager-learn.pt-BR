---
title: Práticas recomendadas para regras de Filtro de tráfego, incluindo regras WAF
description: Saiba mais sobre as práticas recomendadas para regras de Filtro de tráfego, incluindo regras WAF.
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
source-wordcount: '415'
ht-degree: 0%

---


# Práticas recomendadas para regras de filtro de tráfego, incluindo regras WAF

Saiba mais sobre as práticas recomendadas para regras de filtro de tráfego, incluindo regras WAF. É importante observar que as práticas recomendadas descritas neste artigo não são exaustivas e não se destinam a substituir suas próprias políticas e procedimentos de segurança.

## Práticas recomendadas gerais

- Para determinar quais regras são apropriadas para sua organização, colabore com a equipe de segurança.
- Sempre teste regras em ambientes de Desenvolvimento antes de implantá-las em ambientes de Preparo e Produção.
- Ao declarar e validar regras, sempre comece com `action` type `log` para garantir que a regra não esteja bloqueando o tráfego legítimo.
- Para certas regras, a transição de `log` para `block` deve basear-se exclusivamente na análise de um volume suficiente de tráfego no local.
- Introduza regras de forma incremental e considere envolver suas equipes de teste (controle de qualidade, desempenho, teste de penetração) no processo.
- Analise o impacto das regras regularmente usando o [ferramentas do painel](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). Dependendo do volume de tráfego do site, a análise pode ser feita diariamente, semanalmente ou mensalmente.
- Para bloquear o tráfego mal-intencionado que você possa saber após a análise, adicione regras adicionais. Por exemplo, determinados IPs que têm atacado seu site.
- A criação, a implantação e a análise de regras devem ser um processo contínuo e iterativo. Não é uma atividade única.

## Práticas recomendadas para regras de filtro de tráfego

Ative as regras de filtro de tráfego abaixo para seu projeto AEM. No entanto, os valores desejados para `rateLimit` e `clientCountry` As propriedades do devem ser determinadas em colaboração com a equipe de segurança.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>Para seu ambiente de produção, colabore com sua equipe de Segurança da Web para determinar os valores apropriados para `rateLimit`,

## Práticas recomendadas para regras do WAF

Depois que o WAF é licenciado e ativado para o seu programa, os sinalizadores de correspondência de tráfego do WAF aparecem em gráficos e registros de solicitações, mesmo que você não os tenha declarado em uma regra. Dessa forma, você sempre estará ciente do tráfego mal-intencionado potencialmente novo e poderá criar regras conforme necessário. Observe os sinalizadores do WAF que não são refletidos nas regras declaradas e considere declará-los.

Considere as regras do WAF abaixo para o seu projeto AEM. No entanto, os valores desejados para `action` e `wafFlags` A propriedade deve ser determinada em colaboração com a equipe de segurança.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:

    # Traffic Filter rules shown in above section
    ...

    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - SIGSCI-IP
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## Resumo

Em conclusão, este tutorial equipou você com o conhecimento e as ferramentas necessárias para reforçar a segurança de suas aplicações Web no Adobe Experience Manager as a Cloud Service (AEMCS). Com exemplos práticos de regras e insights sobre a análise de resultados, você pode proteger efetivamente seu site e aplicativos.
