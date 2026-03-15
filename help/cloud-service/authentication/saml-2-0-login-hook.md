---
title: Gancho de logon personalizado do SAML 2.0
description: Saiba como desenvolver um gancho de logon SAML 2.0 personalizado para o AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# Gancho de logon do SAML 2.0

Saiba como desenvolver um gancho de logon SAML 2.0 personalizado para o AEM. Este tutorial fornece instruções passo a passo para criar um gancho de logon personalizado que se integra a um provedor de identidade SAML 2.0, permitindo que os usuários se autentiquem usando suas credenciais SAML.

Se o IDP não puder enviar os dados de perfil do usuário e a associação de grupo de usuários na asserção SAML, ou se os dados precisarem ser transformados antes da sincronização com o AEM, ganchos SAML personalizados poderão ser implementados para estender o processo de autenticação SAML. Os ganchos SAML permitem a personalização da atribuição de associação de grupo, a modificação de atributos de perfil do usuário e a adição de lógica de negócios personalizada durante o fluxo de autenticação.

>[!NOTE]
>Ganchos SAML personalizados têm suporte no **AEM as a Cloud Service** e **AEM LTS**. Esse recurso não está disponível em versões mais antigas do AEM.

## Casos de uso comuns

Os ganchos SAML personalizados são úteis quando é necessário:

+ Atribuir dinamicamente a associação de grupo com base na lógica de negócios personalizada além do que é fornecido em asserções SAML
+ Transformar ou enriquecer dados do perfil do usuário antes que ele seja sincronizado com o AEM
+ Mapear estruturas de atributo SAML complexas para propriedades de usuário do AEM
+ Implementar regras de autorização personalizadas ou atribuições de grupos condicionais
+ Adicionar log ou auditoria personalizados durante a autenticação SAML
+ Integrar a sistemas externos durante o processo de autenticação

## Interface de serviço OSGi `SamlHook`

A interface `com.adobe.granite.auth.saml.spi.SamlHook` fornece dois métodos de gancho que são chamados em estágios diferentes do processo de autenticação SAML:

### método `postSamlValidationProcess()`

Este método é chamado **depois** de a resposta SAML ter sido validada, mas **antes** o processo de sincronização de usuário começa. Esse é o local ideal para modificar os dados de asserção SAML, como adicionar ou transformar atributos.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### Casos de uso

+ Adicionar associações de grupo adicionais à asserção
+ Transformar valores de atributo antes que sejam sincronizados
+ Enriquecer a asserção com dados de fontes externas
+ Validar regras de negócios personalizadas

### método `postSyncUserProcess()`

Este método é chamado **após** a conclusão do processo de sincronização do usuário. Este gancho pode ser usado para executar operações adicionais depois que o usuário do AEM é criado ou atualizado.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### Casos de uso

+ Atualizar propriedades adicionais de perfil de usuário não cobertas pela sincronização padrão
+ Criar ou atualizar recursos personalizados relacionados ao usuário no AEM
+ Acionar workflows ou notificações após a autenticação do usuário
+ Registrar eventos de autenticação personalizados

**Importante:** para modificar propriedades de usuário no repositório, a implementação do gancho requer:

+ Uma referência `SlingRepository` inserida via `@Reference`
+ Um [usuário do serviço](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) configurado com as permissões apropriadas (configurado no &quot;Aditamento do Serviço do Mapeador de Usuários do Apache Sling Service&quot;)
+ Gerenciamento adequado de sessão com blocos try-catch-finally

## Implementar um gancho SAML personalizado

As etapas a seguir descrevem como criar e implantar um gancho SAML personalizado.

### Criar a implementação de gancho SAML

Crie uma nova classe Java no projeto AEM que implementa a interface `com.adobe.granite.auth.saml.spi.SamlHook`:

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### Configurar o gancho SAML

O gancho SAML usa a configuração OSGi para especificar a qual IDP ele deve ser aplicado. Crie um arquivo de configuração OSGi no projeto em:

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

O `idpIdentifier` deve corresponder ao valor `idpIdentifier` configurado na configuração de fábrica OSGi do Manipulador de Autenticação SAML correspondente (PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`). Esta correspondência é crítica: o gancho SAML só será chamado para a instância do Manipulador de Autenticação SAML que tem o mesmo valor `idpIdentifier`. O Manipulador de Autenticação SAML é uma configuração de fábrica, o que significa que você pode ter várias instâncias (por exemplo, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`) e cada gancho está vinculado a um manipulador específico através de `idpIdentifier`. A propriedade `service.ranking` controla a ordem de execução quando vários ganchos são configurados (valores mais altos são executados primeiro).

### Adicionar dependências do Maven

Adicione a dependência SAML SPI necessária ao `pom.xml` do projeto principal do AEM Maven.

**Para projetos do AEM as a Cloud Service**, use a dependência da API do AEM SDK, que inclui as interfaces SAML:

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

O artefato `aem-sdk-api` contém todas as interfaces SAML do Adobe Granite necessárias, incluindo `com.adobe.granite.auth.saml.spi.SamlHook`.

### Configurar usuário do serviço (opcional)

Se o gancho SAML precisar modificar o conteúdo no repositório JCR do AEM, como as propriedades do usuário (conforme mostrado no exemplo `postSyncUserProcess`), um [usuário de serviço](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) deve ser configurado:

1. Criar um mapeamento de usuário de serviço no projeto em `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`:

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Crie um script de repoinit para definir o usuário do serviço e as permissões em `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json`:

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

Isso concede ao usuário do serviço permissões para ler e modificar propriedades do usuário no repositório.

### Implantar no AEM

Implante o gancho SAML personalizado no AEM as a Cloud Service:

1. Criar o projeto do AEM
1. Confirme o código no repositório Git do Cloud Manager
1. Implantar usando um pipeline de implantação de Empilhamento completo
1. O gancho SAML será ativado automaticamente quando um usuário se autenticar via SAML


### Considerações importantes

+ **Identificador IDP correspondente**: o `idpIdentifier` configurado no gancho SAML deve corresponder exatamente ao `idpIdentifier` na configuração de fábrica do Manipulador de Autenticação SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)
+ **Nomes de atributo**: verifique se os nomes de atributo referenciados no gancho (por exemplo, `groupMembership`) correspondem aos atributos configurados no Manipulador de Autenticação SAML
+ **Desempenho**: mantém as implementações de gancho leves, pois são executadas durante cada autenticação SAML
+ **Tratamento de erros**: as implementações de gancho SAML devem lançar `com.adobe.granite.auth.saml.spi.SamlHookException` quando ocorrem erros críticos que devem falhar a autenticação. O SAML Authentication Handler capturará essas exceções e retornará `AuthenticationInfo.FAIL_AUTH`. Para operações de repositório, sempre capture `RepositoryException` e registre os erros adequadamente. Usar blocos try-catch-finally para garantir a limpeza adequada dos recursos
+ **Testando**: teste os ganchos personalizados completamente em ambientes inferiores antes de implantar na produção
+ **Vários ganchos**: várias implementações de gancho SAML podem ser configuradas; todos os ganchos correspondentes serão executados. Use a propriedade `service.ranking` no componente OSGi para controlar a ordem de execução (valores de classificação mais altos são executados primeiro). Para reutilizar um gancho SAML em várias configurações de fábrica do Manipulador de autenticação SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`), crie várias configurações de gancho (configurações de fábrica OSGi), cada uma com um `idpIdentifier` diferente que corresponda ao respectivo Manipulador de autenticação SAML
+ **Segurança**: Validar e limpar todos os dados de asserções SAML antes de usá-los na lógica comercial
+ **Acesso ao repositório**: Ao modificar propriedades de usuário em `postSyncUserProcess`, sempre use um [usuário de serviço](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) com as permissões apropriadas em vez de sessões administrativas
+ **Permissões de usuário do serviço**: conceder permissões mínimas necessárias ao [usuário do serviço](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (por exemplo, somente `jcr:read` e `rep:write` em `/home/users`, sem direitos administrativos completos)
+ **Gerenciamento de sessão**: sempre use blocos try-catch-finally para garantir que as sessões de repositório estejam fechadas corretamente, mesmo que ocorram exceções
+ **Tempo de sincronização do usuário**: o gancho `postSyncUserProcess` é executado depois que o usuário é sincronizado com o OAK, portanto, é garantido que o objeto do usuário exista no repositório nesse ponto
