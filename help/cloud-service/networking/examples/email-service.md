---
title: Serviço de e-mail
description: Saiba como configurar o AEM as a Cloud Service para se conectar a um serviço de email usando portas de saída.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# Serviço de e-mail

Enviar emails do AEM as a Cloud Service configurando o AEM `DefaultMailService` para usar portas avançadas de saída de rede.

Como os serviços de email (a maioria) não são executados por HTTP/HTTPS, as conexões com os serviços de email do AEM as a Cloud Service devem ser enviadas por proxy.

+ `smtp.host` está definido como a variável de ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` portanto, é roteado através da saída.
   + `$[env:AEM_PROXY_HOST]` é uma variável reservada que o AEM as a Cloud Service mapeia para o estado interno `proxy.tunnel` host.
   + NÃO tente definir o `AEM_PROXY_HOST` pelo Cloud Manager.
+ `smtp.port` está definido como `portForward.portOrig` porta que mapeia para o host e a porta do serviço de email de destino. Este exemplo usa o mapeamento: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + A variável `smpt.port` está definido como `portForward.portOrig` e NÃO a porta real do servidor SMTP. O mapeamento entre a variável `smtp.port` e a variável `portForward.portOrig` A porta é estabelecida pelo Cloud Manager `portForwards` regra (conforme demonstrado abaixo).

Como os segredos não devem ser armazenados no código, é melhor fornecer o nome de usuário e a senha do serviço de email usando [variáveis de configuração OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), definido usando a CLI da AIO ou a API do Cloud Manager.

Normalmente, [saída de porta flexível](../flexible-port-egress.md) é usado para satisfazer a integração com um serviço de email, a menos que seja necessário `allowlist` o IP Adobe, nesse caso [endereço ip de saída dedicado](../dedicated-egress-ip-address.md) pode ser usado.

Além disso, consulte a documentação do AEM em [envio de e-mail](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## Suporte avançado a rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

Assegure a [apropriado](../advanced-networking.md#advanced-networking) a configuração avançada de rede foi definida antes de seguir este tutorial.

| Sem rede avançada | [Saída de porta flexível](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuração OSGi

Este exemplo de configuração OSGi configura o serviço OSGi de email do AEM para usar um serviço de email externo, por meio do seguinte Cloud Manager `portForwards` regra do [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operação.

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

A variável `EMAIL_USERNAME` e `EMAIL_PASSWORD` A variável OSGi e o segredo podem ser definidos por ambiente, usando:

+ [Configuração do ambiente do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ ou usando o `aio CLI` comando

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
