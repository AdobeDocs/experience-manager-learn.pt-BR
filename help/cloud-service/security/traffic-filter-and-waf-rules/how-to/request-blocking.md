---
title: Restrição do acesso
description: Saiba como restringir o acesso bloqueando solicitações específicas usando regras de filtro de tráfego no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 22%

---

# Restrição do acesso

Saiba como restringir o acesso bloqueando solicitações específicas usando regras de filtro de tráfego no AEM as a Cloud Service.

Este tutorial demonstra como **bloquear solicitações a caminhos internos de IPs públicos** no serviço de Publicação do AEM.

## Por que e quando bloquear solicitações

O bloqueio de tráfego ajuda a aplicar as políticas de segurança organizacional, impedindo o acesso a recursos ou URLs confidenciais em determinadas condições. Em comparação ao registro, o bloqueio é uma ação mais rígida e deve ser usado quando você estiver confiante de que o tráfego de fontes específicas não é autorizado ou não é desejado.

Cenários comuns em que o bloqueio é apropriado incluem:

- Restringindo o acesso a `internal` ou `confidential` páginas somente a intervalos IP internos (por exemplo, atrás de uma VPN corporativa).
- Bloqueio de tráfego de bot, scanners automatizados ou agentes de ameaça identificados por IP ou geolocalização.
- Impedindo o acesso a endpoints obsoletos ou inseguros durante migrações em etapas.
- Limitar o acesso a ferramentas de criação ou rotas de administrador nos níveis de Publicação.

## Pré-requisitos

Antes de continuar, verifique se você concluiu a configuração necessária, conforme descrito no [tutorial Como configurar o filtro de tráfego e as regras do WAF](../setup.md). Além disso, você clonou e implantou o [Projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) no seu ambiente AEM.

## Exemplo: bloquear caminhos internos de IPs públicos

Neste exemplo, você configura uma regra para bloquear o acesso externo a uma página WKND interna, como `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, de endereços IP públicos. Somente usuários dentro de um intervalo IP confiável (como uma VPN corporativa) podem acessar esta página.

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

- Implante as alterações no ambiente do AEM usando o pipeline de configuração do Cloud Manager [criado anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Teste a regra, acessando a página interna do site da WKND, como, por exemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, ou usando o comando CURL abaixo:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita a etapa acima a partir do endereço IP usado na regra e de um endereço IP diferente (por exemplo, usando o seu celular).

## Análise

Para analisar os resultados da regra `block-internal-paths`, siga as mesmas etapas descritas no [tutorial de configuração](../setup.md#cdn-logs-ingestion)

Você deve ver as **Solicitações bloqueadas** e os valores correspondentes nas colunas IP do cliente (cli_ip), host, URL, ação (waf_action) e nome da regra (waf_match).

![Solicitação bloqueada do painel da ferramenta ELK](../assets/how-to/elk-tool-dashboard-blocked.png)
