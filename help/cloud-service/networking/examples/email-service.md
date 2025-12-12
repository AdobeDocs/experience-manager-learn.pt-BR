---
title: Serviço de e-mail
description: Saiba como configurar o AEM as a Cloud Service para se conectar a um serviço de email usando portas de saída.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Serviço de e-mail

Envie emails do AEM as a Cloud Service configurando o `DefaultMailService` do AEM para usar portas avançadas de saída de rede.

Como os serviços de email (a maioria) não são executados por HTTP/HTTPS, as conexões com os serviços de email do AEM as a Cloud Service devem ser enviadas por proxy.

+ `smtp.host` está definido como a variável de ambiente OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` para que seja roteado pela saída.
   + `$[env:AEM_PROXY_HOST]` é uma variável reservada que o AEM as a Cloud Service mapeia para o host `proxy.tunnel` interno.
   + NÃO tente definir o `AEM_PROXY_HOST` via Cloud Manager.
+ `smtp.port` está definido como a porta `portForward.portOrig` que mapeia para o host e a porta do serviço de email de destino. Este exemplo usa o mapeamento: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + O `smpt.port` está definido como a porta `portForward.portOrig`, e NÃO a porta real do servidor SMTP. O mapeamento entre a porta `smtp.port` e a porta `portForward.portOrig` é estabelecido pela regra `portForwards` do Cloud Manager (conforme demonstrado abaixo).

Como os segredos não devem ser armazenados no código, o nome de usuário e a senha do serviço de email devem ser fornecidos com o uso de [variáveis de configuração OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=pt-BR#secret-configuration-values), definidas usando a CLI AIO ou a API do Cloud Manager.

Normalmente, a [saída de porta flexível](../flexible-port-egress.md) é usada para atender à integração com um serviço de email, a menos que seja necessário `allowlist` o IP do Adobe, caso em que o [endereço IP de saída dedicado](../dedicated-egress-ip-address.md) pode ser usado.

Além disso, consulte a documentação do AEM em [enviando email](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=pt-BR#sending-email).

## Suporte avançado a rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

Verifique se a configuração avançada de rede [apropriada](../advanced-networking.md#advanced-networking) foi definida antes de seguir este tutorial.

| Sem rede avançada | [Saída de porta flexível](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede Virtual Privada](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuração OSGi

Este exemplo de configuração OSGi configura o Serviço OSGi de Email do AEM para usar um serviço de email externo, por meio da seguinte regra do Cloud Manager `portForwards` da operação [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration).

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

Configure o [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=pt-BR#sending-email) da AEM conforme exigido pelo seu provedor de email (por exemplo, `smtp.ssl`, etc.).

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

As variáveis OSGi `EMAIL_USERNAME` e `EMAIL_PASSWORD` e o segredo podem ser definidos por ambiente, usando:

+ [Configuração de ambiente do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=pt-BR)
+ ou usando o comando `aio CLI`

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
