---
title: Normalização de solicitações
description: Saiba como normalizar solicitações transformando-as usando regras de filtro de tráfego no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# Normalização de solicitações

Saiba como normalizar solicitações transformando-as usando regras de filtro de tráfego no AEM as a Cloud Service.

## Por que e quando transformar solicitações

As transformações de solicitação são úteis quando você deseja normalizar o tráfego de entrada e reduzir a variação desnecessária causada por parâmetros de consulta ou cabeçalhos desnecessários. Essa técnica é comumente usada para:

- Melhore a eficiência do armazenamento em cache removendo parâmetros de eliminação de cache que não são relevantes para o aplicativo do AEM.
- Proteja a origem contra abusos, minimizando as permutas de solicitações e reduzindo o processamento desnecessário.
- Limpe ou simplifique solicitações antes que sejam encaminhadas ao AEM.

Normalmente, essas transformações são aplicadas na camada de CDN, especialmente para camadas de Publicação do AEM que atendem ao tráfego público.

## Pré-requisitos

Antes de continuar, verifique se você concluiu a configuração necessária, conforme descrito no [tutorial Como configurar o filtro de tráfego e as regras do WAF](../setup.md). Além disso, você clonou e implantou o [Projeto do AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) no seu ambiente AEM.

## Exemplo: parâmetros de consulta não definidos não são necessários para o aplicativo

Neste exemplo, você configura uma regra que **remove todos os parâmetros de consulta, exceto** `search` e `campaignId`, para reduzir a fragmentação do cache.

- Adicione a regra a seguir ao arquivo `/config/cdn.yaml` do projeto da WKND.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- Confirme e envie as alterações ao repositório do Git do Cloud Manager.

- Implante as alterações no ambiente do AEM usando o pipeline de configuração do Cloud Manager [criado anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Teste a regra acessando a página do site WKND, por exemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- Nos logs do AEM (`aemrequest.log`), você deve ver que a solicitação foi transformada em `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`, com a `otherParam` removida.

  ![Transformação de solicitação WKND](../assets/how-to/aemrequest-log-transformation.png)

