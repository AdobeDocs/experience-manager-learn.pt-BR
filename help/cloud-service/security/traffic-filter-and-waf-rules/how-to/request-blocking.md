---
title: Restringir o acesso
description: Saiba como restringir o acesso, bloqueando solicitações específicas com regras de filtro de tráfego no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
exl-id: 53cb8996-4944-4137-a979-6cf86b088d42
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 100%

---

# Restringir o acesso

Saiba como restringir o acesso, bloqueando solicitações específicas com regras de filtro de tráfego no AEM as a Cloud Service.

Este tutorial demonstra como **bloquear solicitações para caminhos internos de IPs públicos** no serviço do AEM Publish.

## Por que e quando bloquear solicitações

O bloqueio do tráfego ajuda a aplicar as políticas de segurança da organização, impedindo o acesso a recursos ou URLs sensíveis em determinadas condições. Em comparação com o registro em log, o bloqueio é uma ação mais rígida e deve ser usado quando você tiver a certeza de que o tráfego de fontes específicas não é autorizado ou não é desejado.

Exemplos de casos comuns em que o bloqueio é apropriado:

- Restringir o acesso a páginas `internal` ou `confidential` somente a intervalos IP internos (por exemplo, por trás de uma VPN corporativa).
- Bloquear o tráfego de bot, leitores automatizados ou agentes de ameaças identificados por IP ou geolocalização.
- Impedir o acesso a pontos de acesso obsoletos ou desprotegidos durante migrações em etapas.
- Limitar o acesso a ferramentas de criação ou rotas do administrador nos níveis de publicação.

## Pré-requisitos

Antes de continuar, certifique-se de ter realizado a configuração necessária, conforme descrito no tutorial [Como configurar as regras de filtro de tráfego e do WAF](../setup.md). Além disso, certifique-se de ter clonado e implantado o [Projeto de sites da WKND no AEM](https://github.com/adobe/aem-guides-wknd) no seu ambiente do AEM.

## Exemplo: bloquear caminhos internos de IPs públicos

Neste exemplo, você configura uma regra para bloquear o acesso externo a uma página interna da WKND, como `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, a partir de endereços IP públicos. Somente usuários dentro de um intervalo de IPs confiáveis (como uma VPN corporativa) podem acessar essa página.

Você pode criar a sua própria página interna (por exemplo, `demo-page.html`) ou usar o [pacote anexado](../assets/how-to/demo-internal-pages-package.zip).

- Adicione a regra a seguir ao arquivo `/config/cdn.yaml` do projeto da WKND.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- Confirme e envie as alterações ao repositório do Git do Cloud Manager.

- Implante as alterações no ambiente do AEM, usando o pipeline de configuração do Cloud Manager [criado anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Teste a regra, acessando a página interna do site da WKND, como, por exemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, ou usando o comando CURL abaixo:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita a etapa acima a partir do endereço IP usado na regra e de um endereço IP diferente (por exemplo, usando o seu celular).

## Análise

Para analisar os resultados da regra `block-internal-paths`, siga as mesmas etapas descritas no [tutorial de configuração](../setup.md#cdn-logs-ingestion)

Você deve ver as **Solicitações bloqueadas** e os valores correspondentes nas colunas de IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match).

![Solicitação bloqueada do painel da ferramenta ELK](../assets/how-to/elk-tool-dashboard-blocked.png)
