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
source-git-commit: d6eddceb3f414e67b5b6e3fba071cd95597dc41c
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Serviço de email

Enviar emails de AEM as a Cloud Service configurando AEM `DefaultMailService` para usar portas avançadas de saída de rede.

Como a maioria dos serviços de email não é executada por HTTP/HTTPS, as conexões com os serviços de email AEM as a Cloud Service devem ser enviadas por proxy.

+ `smtp.host` é definida como a variável de ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` então é roteado através da saída.
   + `$[env:AEM_PROXY_HOST]` é uma variável reservada que AEM mapas as a Cloud Service para o `proxy.tunnel` host.
   + NÃO tente definir a variável `AEM_PROXY_HOST` pelo Cloud Manager.
+ `smtp.port` é definido como `portForward.portOrig` porta que mapeia para o host e a porta do serviço de email de destino. Este exemplo usa o mapeamento: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + O `smpt.port` é definido como `portForward.portOrig` e NÃO a porta real do servidor SMTP. O mapeamento entre a `smtp.port` e `portForward.portOrig` A porta é estabelecida pelo Cloud Manager `portForwards` (conforme demonstrado abaixo).

Como os segredos não devem ser armazenados no código, o nome de usuário e a senha do serviço de email são melhor fornecidos usando [variáveis de configuração secretas do OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), definido usando a AIO CLI ou a API do Cloud Manager.

Normalmente, [saída de porta flexível](../flexible-port-egress.md) é usada para satisfazer a integração com um serviço de email, a menos que seja necessário `allowlist` o Adobe IP, caso em que [endereço ip de saída dedicado](../dedicated-egress-ip-address.md) pode ser usada.

Além disso, reveja AEM documentação sobre [envio de e-mail](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

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
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configurar AEM [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) conforme exigido pelo seu provedor de email (por exemplo, `smtp.ssl`, etc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

O `EMAIL_USERNAME` e `EMAIL_PASSWORD` A variável OSGi e o segredo podem ser definidos por ambiente, usando:

+ [Configuração do ambiente do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ ou usando a `aio CLI` comando

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```
