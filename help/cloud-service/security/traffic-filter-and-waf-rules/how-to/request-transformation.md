---
title: Normalizar solicitações
description: Saiba como normalizar solicitações, transformando-as com regras de filtro de tráfego no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
exl-id: eee81cd6-9090-45d6-b77f-a266de1d9826
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 100%

---

# Normalizar solicitações

Saiba como normalizar solicitações, transformando-as com regras de filtro de tráfego no AEM as a Cloud Service.

## Por que e quando transformar solicitações

As transformações de solicitações são úteis quando você deseja normalizar o tráfego de entrada e reduzir a variância desnecessária causada por parâmetros de consulta ou cabeçalhos desnecessários. Essa técnica é comumente usada para:

- Melhorar a eficiência do armazenamento em cache, removendo parâmetros de saturação de cache que não são relevantes para o aplicativo do AEM.
- Proteger a origem contra abusos, minimizando as permutas de solicitações e reduzindo o processamento desnecessário.
- Limpar ou simplificar as solicitações antes que sejam encaminhadas ao AEM.

Normalmente, essas transformações são aplicadas na camada da CDN, principalmente no caso de níveis do AEM Publish que atendem ao tráfego público.

## Pré-requisitos

Antes de continuar, certifique-se de ter realizado a configuração necessária, conforme descrito no tutorial [Como configurar as regras de filtro de tráfego e do WAF](../setup.md). Além disso, certifique-se de ter clonado e implantado o [Projeto de sites da WKND no AEM](https://github.com/adobe/aem-guides-wknd) no seu ambiente do AEM.

## Exemplo: parâmetros de consulta não definidos desnecessários para o aplicativo

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

- Implante as alterações no ambiente do AEM, usando o pipeline de configuração do Cloud Manager [criado anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Para testar a regra, acesse a página do site da WKND, como, por exemplo, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- Nos logs do AEM (`aemrequest.log`), você deve ver que a solicitação foi transformada em `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`, com o `otherParam` removido.

  ![Transformação de solicitações da WKND](../assets/how-to/aemrequest-log-transformation.png)
