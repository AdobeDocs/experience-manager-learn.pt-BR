---
title: Serviço de email
description: Saiba como configurar AEM as a Cloud Service para se conectar a um serviço de email usando portas de saída.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---

# Serviço de email

Enviar emails de AEM as a Cloud Service configurando AEM `DefaultMailService` para usar portas avançadas de saída de rede.

Como a maioria dos serviços de email não é executada por HTTP/HTTPS, as conexões com os serviços de email AEM as a Cloud Service devem ser enviadas por proxy.

+ `smtp.host` é definida como a variável de ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` então é roteado através da saída.
+ `smtp.port` é definido como `portForward.portOrig` porta que mapeia para o host e a porta do serviço de email de destino. Este exemplo usa o mapeamento: `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.

Como os segredos não devem ser armazenados no código, o nome de usuário e a senha do serviço de email são melhor fornecidos usando [variáveis de configuração secretas do OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), definido usando a AIO CLI ou a API do Cloud Manager.

Normalmente, [saída de porta flexível](../flexible-port-egress.md) é usada para satisfazer a integração com um serviço de email, a menos que seja necessário `allowlist` o Adobe IP, caso em que [endereço ip de saída dedicado](../dedicated-egress-ip-address.md) pode ser usada.

## Suporte avançado para rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

Verifique se a variável [adequada](../advanced-networking.md#advanced-networking) a configuração avançada de rede foi configurada antes de seguir este tutorial.

| Sem rede avançada | [Saída flexível da porta](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuração do OSGi

Este exemplo de configuração OSGi configura AEM Serviço OSGi de Email para usar um serviço de email externo, por meio do seguinte Cloud Manager `portForwards` regra do [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operação.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configurar AEM [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) conforme exigido pelo seu provedor de email (por exemplo, `smtp.ssl`, etc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=emailapikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

O seguinte `aio CLI` pode ser usado para definir os segredos do OSGi com base no ambiente:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```
