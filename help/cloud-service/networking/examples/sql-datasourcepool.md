---
title: Conexões SQL usando DataSourcePool do JDBC
description: Saiba como se conectar a bancos de dados SQL do AEM as a Cloud Service usando as portas JDBC DataSourcePool e de saída da AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 117
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 0%

---

# Conexões SQL usando DataSourcePool do JDBC

As conexões com bancos de dados SQL (e outros serviços não HTTP/HTTPS) devem ser enviadas por proxy da AEM, incluindo aquelas feitas usando o serviço OSGi DataSourcePool da AEM para gerenciar as conexões.

## Suporte avançado a rede

O código de exemplo a seguir é suportado pelas seguintes opções avançadas de rede.

Verifique se a configuração avançada de rede [apropriada](../advanced-networking.md#advanced-networking) foi definida antes de seguir este tutorial.

| Sem rede avançada | [Saída de porta flexível](../flexible-port-egress.md) | [Endereço IP de saída dedicado](../dedicated-egress-ip-address.md) | [Rede Virtual Privada](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuração OSGi

A cadeia de conexão da configuração do OSGi usa:

+ Valor `AEM_PROXY_HOST` por meio da [variável de ambiente de configuração OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=pt-BR#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` como o host da conexão
+ `30001` que é o valor `portOrig` para o mapeamento de encaminhamento de porta do Cloud Manager `30001` → `mysql.example.com:3306`

Como os segredos não devem ser armazenados no código, o nome de usuário e a senha da conexão SQL são fornecidos melhor por meio de variáveis de configuração OSGi, definidas usando a CLI AIO ou as APIs do Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

O seguinte comando `aio CLI` pode ser usado para definir os segredos OSGi por ambiente:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Exemplo de código

Este exemplo de código Java™ é de um serviço OSGi que faz uma conexão com um banco de dados MySQL externo por meio do serviço OSGi DataSourcePool da AEM.
A configuração de fábrica OSGi de DataSourcePool especifica uma porta (`30001`) que é mapeada por meio da regra `portForwards` na operação [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) para o host e a porta externos, `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
    }
}
```

## Dependências do driver MySQL

A AEM as a Cloud Service geralmente exige que você forneça drivers de banco de dados Java™ para oferecer suporte às conexões. Normalmente, a melhor maneira de obter os drivers é incorporar os artefatos do pacote OSGi que contêm esses drivers ao projeto do AEM por meio do pacote `all`.

### Reator pom.xml

Inclua as dependências do driver do banco de dados no reator `pom.xml` e depois faça referência a elas nos subprojetos `all`.

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## Todos os pom.xml

Incorpore os artefatos de dependência do driver do banco de dados no pacote `all` para que eles sejam implantados e estejam disponíveis no AEM as a Cloud Service. Esses artefatos __devem__ ser pacotes OSGi que exportam a classe Java™ do driver do banco de dados.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
