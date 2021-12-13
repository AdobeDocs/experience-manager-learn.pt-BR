---
title: Saída flexível da porta
description: Saiba como configurar e usar portas flexíveis para suportar conexões externas de AEM as a Cloud Service a serviços externos.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '1035'
ht-degree: 0%

---

# Saída flexível da porta

Saiba como configurar e usar portas flexíveis para suportar conexões externas de AEM as a Cloud Service a serviços externos.

## O que é saída de porta flexível?

O acesso flexível da porta permite que regras personalizadas e específicas de encaminhamento da porta sejam anexadas a AEM as a Cloud Service, permitindo que sejam feitas conexões de AEM a serviços externos.

Um programa do Cloud Manager só pode ter um __individual__ tipo de infraestrutura de rede. Certifique-se de que o endereço IP de saída dedicado seja o mais [tipo adequado de infraestrutura de rede](./advanced-networking.md)  para seu AEM as a Cloud Service antes de executar os seguintes comandos.

>[!MORELIKETHIS]
>
> Leia o AEM as a Cloud Service [documentação de configuração de rede avançada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#flexible-port-egress) para obter mais detalhes sobre a saída flexível do porto.

## Pré-requisitos

Os itens a seguir são necessários ao configurar saídas flexíveis da porta:

+ Projeto do Adobe I/O com a API do Cloud Manager ativada e [Permissões de Proprietário comercial do Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Acesso ao [Credenciais de autenticação da API do Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como chave da API)
   + Token de acesso (também conhecido como Token portador)
+ A ID do programa do Cloud Manager
+ As IDs do ambiente do Cloud Manager

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. O `curl` Os comandos assumem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o `\` caractere de quebra de linha com `^`.

## Habilitar saída de porta flexível por programa

Comece ativando o acesso flexível da porta AEM as a Cloud Service.

1. Primeiro, determine a região em que a Rede avançada será configurada usando a API do Cloud Manager. [listRegiões](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) operação. O `region name` será necessário fazer chamadas de API subsequentes do Cloud Manager. Normalmente, a região em que o ambiente de Produção reside é usada.

   __solicitação HTTP listRegiões__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Habilitar saída de porta flexível para um Programa Cloud Manager usando a API do Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) operação. Use o `region` código obtido da API do Cloud Manager `listRegions` operação.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Aguarde 15 minutos para que o Programa Cloud Manager forneça a infraestrutura de rede.

1. Verifique se o ambiente foi concluído __saída de porta flexível__ configuração usando a API do Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) , usando a `id` retornado da solicitação HTTP createNetworkInfrastructure na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém uma __status__ de __ready__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

## Configurar proxies flexíveis de saída de porta por ambiente

1. Ative e configure a variável __saída de porta flexível__ configuração em cada ambiente AEM as a Cloud Service usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operação.

   __solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Defina os parâmetros JSON em um `flexible-port-egress.json` e fornecidos por via de curl `... -d @./flexible-port-egress.json`.

[Baixe o exemplo flexível-port-egress.json](./assets/flexible-port-egress.json)

   ```json
   {
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   Para cada `portForwards` , a rede avançada define a seguinte regra de encaminhamento:

   | Host proxy | Porta proxy |  | Host externo | Porta externa |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se sua implantação de AEM __only__ exige conexões HTTP/HTTPS (porta 80/443) para o serviço externo, deixe a variável `portForwards` matriz vazia, pois essas regras são necessárias apenas para solicitações não HTTP/HTTPS.

1. Para cada ambiente, valide se as regras de saída estão em vigor usando a API do Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) operação.

   __solicitação HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Configurações flexíveis de saída de porta podem ser atualizadas usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operação. Lembrar `enableEnvironmentAdvancedNetworkingConfiguration` é um `PUT` , portanto, todas as regras devem ser fornecidas com cada invocação desta operação.

1. Agora você pode usar a configuração flexível de saída da porta em seu código e configuração de AEM personalizados.


## Ligação a serviços externos através de portas flexíveis

Com o proxy de saída de porta flexível ativado, AEM código e configuração podem usá-los para fazer chamadas para serviços externos. Há dois sabores de chamadas externas que AEM tratam de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos em portas não padrão
   + Inclui chamadas HTTP/HTTPS feitas para serviços em execução em portas diferentes das portas 80 ou 443 padrão.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

Por padrão, as solicitações HTTP/HTTPS de AEM em portas padrão (80/443) são permitidas e não precisam de configuração ou considerações adicionais.


### HTTP/HTTPS em portas não padrão

Ao criar conexões HTTP/HTTPS com portas não padrão (não-80/443) a partir de AEM, as conexões devem ser feitas por meio de host e portas especiais, fornecidas por meio de espaços reservados.

AEM fornece dois conjuntos de variáveis especiais do sistema Java™ que mapeiam para AEM proxies HTTP/HTTPS.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy para ambas as conexões HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | Porta proxy para conexões HTTPS (defina o fallback para `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | Porta proxy para conexões HTTPS (defina o fallback para `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Ao fazer chamadas HTTP/HTTPS para serviços externos em portas não padrão, nenhum `portForwards` deve ser definido usando a API do Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , já que as &quot;regras&quot; de encaminhamento de porta são definidas como &quot;em código&quot;.

>[!TIP]
>
> Consulte AEM documentação de saída de porta flexível do as a Cloud Service para [o conjunto completo de regras de roteamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Exemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS em portas não padrão" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS em portas não padrão</a></strong></div>
    <p>
        Exemplo de código Java™ tornando a conexão HTTP/HTTPS de AEM as a Cloud Service para um serviço externo em portas HTTP/HTTPS não padrão.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Conexões não HTTP/HTTPS para serviços externos

Ao criar conexões não HTTP/HTTPS (por exemplo, SQL, SMTP e assim por diante) do AEM, a conexão deve ser feita por meio de um nome de host especial fornecido pelo AEM.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy para conexões não HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


As conexões com serviços externos são então chamadas por meio da variável `AEM_PROXY_HOST` e a porta mapeada (`portForwards.portOrig`), que AEM então roteia para o nome de host externo mapeado (`portForwards.name`) e a porta (`portForwards.portDest`).

| Host proxy | Porta proxy |  | Host externo | Porta externa |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexão SQL usando JDBC DataSourcePool</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos configurando AEM pool de fonte de dados JDBC.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexão SQL usando APIs Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexão SQL usando APIs Java™</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos usando as APIs SQL do Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="VPN (Virtual Private Network)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Serviço de email</a></strong></div>
      <p>
        Exemplo de configuração do OSGi usando AEM para se conectar a serviços de email externos.
      </p>
    </td>   
</tr></table>
