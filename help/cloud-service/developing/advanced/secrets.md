---
title: Gerenciar segredos no AEM as a Cloud Service
description: Conheça as práticas recomendadas para gerenciar segredos no AEM as a Cloud Service, usando ferramentas e técnicas fornecidas pelo AEM para proteger suas informações confidenciais, garantindo que seu aplicativo permaneça seguro e confidencial.
version: Cloud Service
topic: Development, Security
feature: OSGI, Cloud Manager
role: Developer
jira: KT-15880
level: Intermediate, Experienced
source-git-commit: ba7ba62dbc70b07f73d96a488e7d3088bf9d467a
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Gerenciamento de segredos no AEM as a Cloud Service

O gerenciamento de segredos, como chaves de API e senhas, é fundamental para manter a segurança do aplicativo. O Adobe Experience Manager (AEM) as a Cloud Service ferramentas robustas para manipular segredos com segurança.

Neste tutorial, você aprenderá as práticas recomendadas para gerenciar segredos no AEM. Abordaremos as ferramentas e técnicas fornecidas pelo AEM para proteger suas informações confidenciais, garantindo que seu aplicativo permaneça seguro e confidencial.

Este tutorial presume um conhecimento prático do desenvolvimento em Java do AEM, serviços OSGi, Modelos Sling e Adobe Cloud Manager.

## Serviço OSGi do gerenciador de segredos

No AEM as a Cloud Service, o gerenciamento de segredos por meio dos serviços OSGi fornece uma abordagem escalável e segura. Os serviços OSGi podem ser configurados para lidar com informações confidenciais, como chaves e senhas de API, definidas por meio de configurações OSGi e definidas pelo Cloud Manager.

### Implementação do serviço OSGi

Vamos analisar o desenvolvimento de um serviço OSGi personalizado que [expõe segredos das configurações OSGi](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values).

A implementação lê segredos da configuração OSGi por meio do método `@Activate` e os expõe por meio do método `getSecret(String secretName)`. Como alternativa, você pode criar métodos discretos como `getApiKey()` para cada segredo, mas essa abordagem requer mais manutenção à medida que segredos são adicionados ou removidos.

```java
package com.example.core.util.impl;

import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.apache.sling.api.resource.ValueMap;
import org.apache.sling.api.resource.ValueMapDecorator;
import java.util.Map;

@Component(
    service = { SecretsManager.class }
)
public class SecretsManagerImpl implements SecretsManager {
    private static final Logger log = LoggerFactory.getLogger(SecretsManagerImpl.class);
 
    private ValueMap secrets;

    @Override
    public String getSecret(String secretName) {
        return secrets.get(secretName, String.class);
    }

    @Activate
    @Modified
    protected void activate(Map<String, Object> properties) {
        secrets = new ValueMapDecorator(properties);
    }
}
```

Como um serviço OSGi, é melhor registrá-lo e consumi-lo por meio de uma interface Java. Abaixo está uma interface simples que permite aos consumidores obter segredos por nome de propriedade OSGi.

```java
package com.example.core.util;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface SecretsManager {
    String getSecret(String secretName);
}
```

## Mapeamento de segredos para a configuração do OSGi

Para expor valores secretos no serviço OSGi, mapeie-os para configurações OSGi usando [valores de configuração secreta OSGi](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values). Defina o nome da propriedade OSGi como a chave para recuperar o valor secreto do método `SecretsManager.getSecret()`.

Defina os segredos no arquivo de configuração OSGi `/apps/example/osgiconfig/config/com.example.core.util.impl.SecretsManagerImpl.cfg.json` em seu projeto AEM Maven. Cada propriedade representa um segredo exposto no AEM, com o valor definido pelo Cloud Manager. A chave é o nome da propriedade OSGi, usada para recuperar o valor secreto do serviço `SecretsManager`.

```json
{
    "api.key": "$[secret:api_key]",
    "service.password": "$[secret:service_password]"
}
```

Como alternativa ao uso de um serviço OSGi do gerenciador de segredos compartilhados, você pode incluir segredos diretamente na configuração OSGi de serviços específicos que os utilizam. Essa abordagem é útil quando os segredos são necessários apenas para um único serviço OSGi e não são compartilhados entre vários serviços. Nesse caso, os valores secretos são definidos no arquivo de configuração OSGi para o serviço específico e acessados no código Java do serviço através do método `@Activate`.

## Consumir segredos

Os segredos podem ser consumidos do serviço OSGi de várias maneiras, como a partir de um Modelo Sling ou de outro serviço OSGi. Abaixo estão exemplos de como consumir segredos de ambos.

### Do modelo Sling

Os Modelos Sling geralmente fornecem lógica de negócios para componentes do site AEM. O serviço OSGi `SecretsManager` pode ser consumido por meio da anotação `@OsgiService` e usado no Modelo Sling para recuperar o valor secreto.

```java
import com.example.core.util.SecretsManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.OsgiService;

@Model(
    adaptables = {SlingHttpServletRequest.class, Resource.class},
    adapters = {ExampleDatabaseModel.class}
)
public class ExampleDatabaseModelImpl implements ExampleDatabaseModel {

    @OsgiService
    SecretsManager secretsManager;

    @Override 
    public String doWork() {
        final String secret = secretsManager.getSecret("api.key");
        // Do work with secret
    }
}
```

### Do serviço OSGi

Os serviços OSGi geralmente expõem a lógica de negócios reutilizável no AEM, usada por Modelos Sling, serviços AEM, como Workflows ou outros serviços OSGi personalizados. O serviço OSGi `SecretsManager` pode ser consumido por meio da anotação `@Reference` e usado no serviço OSGi para recuperar o valor secreto.

```java
import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component
public class ExampleSecretConsumerImpl implements ExampleSecretConsumer {

    @Reference
    SecretsManager secretsManager;

    public void doWork() {
        final String secret = secretsManager.getSecret("service.password");
        // Do work with the secret
    }
}
```

## Configuração de segredos no Cloud Manager

Com o serviço e a configuração do OSGi em vigor, a etapa final é definir os valores secretos no Cloud Manager.

Os valores de segredos podem ser definidos por meio da [API do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#tag/Variables) ou, mais comumente, pela [Interface do usuário do Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#overview). Para aplicar uma variável secreta por meio da interface do Cloud Manager:

![Configuração de Segredos do Cloud Manager](./assets/secrets/cloudmanager-configuration.png)

1. Faça logon no [Adobe Cloud Manager](https://my.cloudmanager.adobe.com).
1. Selecione o Programa AEM e o Ambiente para o qual deseja definir o segredo.
1. Na exibição de detalhes do Ambiente, selecione a guia **Configuração**.
1. Selecione **Adicionar**.
1. Na caixa de diálogo Configuração do ambiente:
   - Insira o nome da variável secreta (por exemplo, `api_key`) referenciada na configuração OSGi.
   - Insira o valor do segredo.
   - Selecione a qual serviço AEM o segredo se aplica.
   - Selecione **Segredo** como o tipo.
1. Selecione **Adicionar** para manter o segredo.
1. Adicione quantos segredos forem necessários. Quando terminar, selecione **Salvar** para aplicar as alterações imediatamente ao ambiente AEM.

Usar as configurações do Cloud Manager para segredos oferece a vantagem de aplicar valores diferentes a ambientes ou serviços diferentes e girar segredos sem reimplantar o aplicativo AEM.
