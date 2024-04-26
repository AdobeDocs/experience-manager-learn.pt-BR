---
title: Saída de porta flexível
description: Saiba como configurar e usar saída de porta flexível para suportar conexões externas do AEM as a Cloud Service para serviços externos.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
last-substantial-update: 2024-04-26T00:00:00Z
duration: 906
source-git-commit: 4e3f77a9e687042901cd3b175d68a20df63a9b65
workflow-type: tm+mt
source-wordcount: '1280'
ht-degree: 2%

---

# Saída de porta flexível

Saiba como configurar e usar saída de porta flexível para suportar conexões externas do AEM as a Cloud Service para serviços externos.

## O que é saída de porta flexível?

Saída de porta flexível permite que regras de encaminhamento de porta personalizadas e específicas sejam anexadas ao AEM as a Cloud Service, permitindo que conexões do AEM a serviços externos sejam feitas.

Um programa do Cloud Manager só pode ter um __solteiro__ tipo de infraestrutura de rede. Garantir que a saída de porta flexível seja a mais [tipo adequado de infraestrutura de rede](./advanced-networking.md) para o AEM as a Cloud Service antes de executar os seguintes comandos.

>[!MORELIKETHIS]
>
> Leia o as a Cloud Service do AEM [documentação avançada de configuração de rede](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) para obter mais detalhes sobre saída de porta flexível.


## Pré-requisitos

Os itens a seguir são necessários ao definir ou definir a saída de porta flexível usando as APIs do Cloud Manager:

+ Projeto do Adobe Developer Console com a API do Cloud Manager ativada e [Permissões do proprietário da empresa no Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acesso a [Credenciais de autenticação da API do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como Chave de API)
   + Token de acesso (também conhecido como Token do portador)
+ A ID do programa do Cloud Manager
+ As IDs de ambiente do Cloud Manager

Para obter mais detalhes, assista à seguinte apresentação de como configurar e obter credenciais da API do Cloud Manager e como usá-las para fazer uma chamada de API do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. Os dados fornecidos `curl` assumem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o `\` caractere de quebra de linha com `^`.


## Habilitar saída de porta flexível por programa

Comece ativando a saída de porta flexível no AEM as a Cloud Service.

>[!BEGINTABS]

>[!TAB Cloud Manager]

A saída de porta flexível pode ser ativada usando o Cloud Manager. As etapas a seguir descrevem como ativar a saída de porta flexível no AEM as a Cloud Service usando o Cloud Manager.

1. Faça logon no [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) como Proprietário da empresa do Cloud Manager.
1. Navegue até o Programa desejado.
1. No menu esquerdo, navegue até __Serviços > Infraestrutura de rede__.
1. Selecione o __Adicionar infraestrutura de rede__ botão.

   ![Adicionar infraestrutura de rede](./assets/cloud-manager__add-network-infrastructure.png)

1. No __Adicionar infraestrutura de rede__ , selecione a __Saída de porta flexível__ e selecione a opção __Região__ para criar o endereço IP de saída dedicado.

   ![Adicionar saída de porta flexível](./assets/flexible-port-egress/select-type.png)

1. Selecionar __Salvar__ para confirmar a adição da saída de porta flexível.

   ![Confirmar criação de saída de porta flexível](./assets/flexible-port-egress/confirmation.png)

1. Aguardar a infraestrutura de rede ser criada e marcada como __Pronto__. Esse processo pode levar até 1 hora.

   ![Status de criação de saída de porta flexível](./assets/flexible-port-egress/ready.png)

Com a saída de porta flexível criada, agora é possível configurar as regras de encaminhamento de porta usando as APIs do Cloud Manager, conforme descrito abaixo.

>[!TAB APIs do Cloud Manager]

A saída de porta flexível pode ser ativada usando as APIs do Cloud Manager. As etapas a seguir descrevem como ativar a saída de porta flexível no AEM as a Cloud Service usando a API do Cloud Manager.

1. Primeiro, determine a região em que a Rede avançada é configurada no usando a API do Cloud Manager [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. A variável `region name` O é necessário para fazer chamadas de API subsequentes do Cloud Manager. Normalmente, a região em que o ambiente de Produção reside é usada.

   Encontre a região do seu ambiente as a Cloud Service AEM em [Cloud Manager](https://my.cloudmanager.adobe.com) no [detalhes do ambiente](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). O nome da região exibido no Cloud Manager pode ser [mapeado para o código de região](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) usada na API do Cloud Manager.

   __solicitação HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. Ativar saída de porta flexível para um programa do Cloud Manager usando a API do Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. Use o `region` código obtido da API do Cloud Manager `listRegions` operação.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Aguarde 15 minutos para que o programa do Cloud Manager provisione a infraestrutura de rede.

3. Verifique se o ambiente terminou __saída de porta flexível__ configuração usando a API do Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) operação, utilizando o `id` retornado do `createNetworkInfrastructure` Solicitação HTTP na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém um __status__ de __pronto__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

Com a saída de porta flexível criada, agora é possível configurar as regras de encaminhamento de porta usando as APIs do Cloud Manager, conforme descrito abaixo.

>[!ENDTABS]

## Configuração de proxies de saída de porta flexíveis por ambiente

1. Habilite e configure o __saída de porta flexível__ configuração em cada ambiente do AEM as a Cloud Service usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação.

   __solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Defina os parâmetros JSON em uma `flexible-port-egress.json` e fornecido para curl via `... -d @./flexible-port-egress.json`.

   [Baixe o exemplo de Flex-port-egress.json](./assets/flexible-port-egress.json). Este arquivo é apenas um exemplo. Configure seu arquivo conforme necessário com base nos campos opcionais/obrigatórios documentados em [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   Para cada `portForwards` de rede avançada define a seguinte regra de encaminhamento:

   | Host do proxy | Porta do proxy |  | Host externo | Porta externa |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se a sua implantação do AEM __somente__ exige conexões HTTP/HTTPS (porta 80/443) para o serviço externo, deixe o `portForwards` matriz vazia, pois essas regras são necessárias somente para solicitações não HTTP/HTTPS.

1. Para cada ambiente, valide se as regras de saída estão em vigor usando a API do Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação.

   __solicitação HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Configurações flexíveis de saída de porta podem ser atualizadas usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. Lembrar `enableEnvironmentAdvancedNetworkingConfiguration` é um `PUT` para que todas as regras sejam fornecidas com cada chamada desta operação.

1. Agora, você pode usar a configuração flexível de saída de porta em seu código e configuração personalizados do AEM.


## Conexão com serviços externos por meio de saída de porta flexível

Com o proxy de saída de porta flexível ativado, o código e a configuração do AEM podem usá-los para fazer chamadas para serviços externos. Há duas opções de chamadas externas que o AEM trata de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos em portas fora do padrão
   + Inclui chamadas HTTP/HTTPS feitas para serviços executados em portas diferentes das portas padrão 80 ou 443.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

As solicitações HTTP/HTTPS do AEM em portas padrão (80/443) são permitidas por padrão e não precisam de configuração ou considerações extras.


### HTTP/HTTPS em portas fora do padrão

Ao criar conexões HTTP/HTTPS com portas fora do padrão (não-80/443) do AEM, as conexões devem ser feitas por meio de hosts e portas especiais, fornecidas por meio de espaços reservados.

O AEM fornece dois conjuntos de variáveis especiais do sistema Java™ que são mapeadas para proxies HTTP/HTTPS do AEM.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy para conexões HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | Porta proxy para conexões HTTPS (defina o fallback como `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | Porta proxy para conexões HTTPS (defina o fallback como `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Ao fazer chamadas HTTP/HTTPS para serviços externos em portas fora do padrão, nenhuma `portForwards` deve ser definido usando a API do Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` operação, já que as &quot;regras&quot; de encaminhamento de porta são definidas &quot;no código&quot;.

>[!TIP]
>
> Consulte a documentação de saída de porta flexível do AEM as a Cloud Service para [o conjunto completo de regras de roteamento](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).

#### Exemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS em portas fora do padrão" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS em portas fora do padrão</a></strong></div>
    <p>
        Exemplo de código Java™ que faz a conexão HTTP/HTTPS do AEM as a Cloud Service para um serviço externo em portas HTTP/HTTPS não padrão.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Conexões não HTTP/HTTPS com serviços externos

Ao criar conexões não HTTP/HTTPS (por exemplo, SQL, SMTP e assim por diante) do AEM, a conexão deve ser feita por meio de um nome de host especial fornecido pelo AEM.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy para conexões não HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


As conexões com serviços externos são então chamadas por meio do `AEM_PROXY_HOST` e a porta mapeada (`portForwards.portOrig`), que o AEM roteia para o nome de host externo mapeado (`portForwards.name`) e porta (`portForwards.portDest`).

| Host do proxy | Porta do proxy |  | Host externo | Porta externa |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexão SQL usando JDBC DataSourcePool</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos configurando o pool de fontes de dados JDBC do AEM.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexão SQL usando APIs Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexão SQL usando APIs Java™</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos usando APIs SQL do Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="VPN (Virtual Private Network)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Serviço de e-mail</a></strong></div>
      <p>
        Exemplo de configuração OSGi usando AEM para se conectar a serviços de email externos.
      </p>
    </td>   
</tr></table>
