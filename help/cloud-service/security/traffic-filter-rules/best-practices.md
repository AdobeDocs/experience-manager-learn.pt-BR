---
title: Práticas recomendadas para regras de Filtro de tráfego incluindo regras do WAF
description: Saiba mais sobre as práticas recomendadas para regras de Filtro de tráfego incluindo regras do WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '411'
ht-degree: 100%

---

# Práticas recomendadas para regras de filtro de tráfego incluindo regras do WAF

Saiba mais sobre as práticas recomendadas para regras de filtro de tráfego incluindo regras do WAF. É importante observar que as práticas recomendadas descritas neste artigo não são completas e não se destinam a substituir suas próprias políticas e procedimentos de segurança.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Práticas recomendadas gerais

- Para determinar quais regras são apropriadas para sua organização, colabore com a equipe de segurança.
- Sempre teste regras em ambientes de desenvolvimento antes de implantá-las em ambientes de preparo e produção.
- Ao declarar e validar regras, sempre comece com `action` tipo `log` para garantir que a regra não esteja bloqueando o tráfego legítimo.
- Para determinadas regras, a transição de `log` para `block` deve ser puramente baseada na análise de tráfego suficiente do site.
- Introduza regras de forma incremental e considere envolver suas equipes de teste (controle de qualidade, desempenho, teste de penetração) no processo.
- Analise o impacto das regras regularmente usando as [ferramentas do painel](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Dependendo do volume de tráfego do site, a análise pode ser feita de forma diária, semanal e mensal.
- Para bloquear tráfego mal-intencionado do qual você possa tomar conhecimento após a análise, adicione regras adicionais. Por exemplo, determinados IPs que têm atacado seu site.
- A criação, a implantação e a análise de regras devem ser um processo contínuo e iterativo. Não é uma atividade única.

## Práticas recomendadas para regras de filtro de tráfego

Habilite as regras de filtro de tráfego abaixo para seu projeto do AEM. No entanto, os valores desejados para as propriedades `rateLimit` e `clientCountry` devem ser determinados em colaboração com a equipe de segurança.

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
>Para seu ambiente de produção, colabore com sua equipe de Segurança da Web para determinar os valores apropriados para `rateLimit`

## Práticas recomendadas para regras do WAF

Depois que o WAF é licenciado e habilitado para o seu programa, os sinalizadores de correspondência de tráfego do WAF aparecem em gráficos e logs de solicitação, mesmo que você não os tenha declarado em uma regra. Portanto, você está sempre ciente do tráfego potencialmente mal-intencionado novo e pode criar regras conforme necessário. Examine os sinalizadores do WAF que não são refletidos nas regras declaradas e considere declará-los.

Considere as regras do WAF abaixo para seu projeto do AEM. No entanto, os valores desejados para as propriedades `action` e `wafFlags` devem ser determinados em colaboração com a equipe de segurança.

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

Em conclusão, este tutorial preparou você com o conhecimento e as ferramentas necessárias para reforçar a segurança dos seus aplicativos Web no Adobe Experience Manager as a Cloud Service (AEMCS). Com exemplos práticos de regras e insights sobre a análise de resultados, você pode proteger efetivamente seu site e aplicativos.



