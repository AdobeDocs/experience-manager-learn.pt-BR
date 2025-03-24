---
title: Saída de porta flexível
description: Saiba como configurar e usar saída de porta flexível para suportar conexões externas do AEM as a Cloud Service com serviços externos.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
last-substantial-update: 2024-04-26T00:00:00Z
duration: 870
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 2%

---

# Saída de porta flexível

Saiba como configurar e usar saída de porta flexível para suportar conexões externas do AEM as a Cloud Service com serviços externos.

## O que é saída de porta flexível?

Saída de porta flexível permite que regras específicas e personalizadas de encaminhamento de porta sejam conectadas ao AEM as a Cloud Service, permitindo que sejam feitas conexões do AEM com serviços externos.

Um Programa Cloud Manager só pode ter um tipo de infraestrutura de rede __único__. Certifique-se de que a saída de porta flexível seja o [tipo mais apropriado de infraestrutura de rede](./advanced-networking.md) para o AEM as a Cloud Service antes de executar os comandos a seguir.

>[!MORELIKETHIS]
>
> Leia a [documentação avançada de configuração de rede](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) do AEM as a Cloud Service para obter mais detalhes sobre saída de porta flexível.


## Pré-requisitos

Os itens a seguir são necessários ao definir ou configurar a saída de porta flexível usando APIs do Cloud Manager:

+ Projeto do Adobe Developer Console com API do Cloud Manager habilitada e [permissões de Proprietário da empresa do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acesso a [credenciais de autenticação da API do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como Chave de API)
   + Token de acesso (também conhecido como Token do portador)
+ A ID do programa Cloud Manager
+ As IDs de ambiente do Cloud Manager

Para obter mais detalhes [revise como instalar, configurar e obter credenciais da API do Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth), para usá-las para fazer uma chamada de API do Cloud Manager.

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. Os comandos `curl` fornecidos pressupõem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o caractere de quebra de linha `\` por `^`.


## Habilitar saída de porta flexível por programa

Comece ativando a saída de porta flexível no AEM as a Cloud Service.

>[!BEGINTABS]

>[!TAB Cloud Manager]

Saída de porta flexível pode ser ativada usando o Cloud Manager. As etapas a seguir descrevem como ativar a saída de porta flexível no AEM as a Cloud Service usando o Cloud Manager.

1. Faça logon no [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) como Proprietário da empresa do Cloud Manager.
1. Navegue até o Programa desejado.
1. No menu esquerdo, navegue até __Serviços > Infraestrutura de Rede__.
1. Selecione o botão __Adicionar infraestrutura de rede__.

   ![Adicionar infraestrutura de rede](./assets/cloud-manager__add-network-infrastructure.png)

1. Na caixa de diálogo __Adicionar infraestrutura de rede__, selecione a opção __Saída flexível da porta__ e selecione a __Região__ para criar o endereço IP de saída dedicado.

   ![Adicionar saída de porta flexível](./assets/flexible-port-egress/select-type.png)

1. Selecione __Salvar__ para confirmar a adição da saída de porta flexível.

   ![Confirmar criação de saída de porta flexível](./assets/flexible-port-egress/confirmation.png)

1. Aguarde a infraestrutura de rede ser criada e marcada como __Pronta__. Esse processo pode levar até 1 hora.

   ![Status de criação de saída de porta flexível](./assets/flexible-port-egress/ready.png)

Com a saída de porta flexível criada, agora é possível configurar as regras de encaminhamento de porta usando as APIs do Cloud Manager, conforme descrito abaixo.

>[!TAB APIs do Cloud Manager]

A saída flexível da porta pode ser ativada usando as APIs do Cloud Manager. As etapas a seguir descrevem como ativar a saída de porta flexível no AEM as a Cloud Service usando a API do Cloud Manager.

1. Primeiro, determine a região em que a Rede avançada está configurada usando a operação [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager. O `region name` é necessário para fazer chamadas de API do Cloud Manager subsequentes. Normalmente, a região em que o ambiente de Produção reside é usada.

   Encontre a região do seu ambiente do AEM as a Cloud Service em [Cloud Manager](https://my.cloudmanager.adobe.com) nos [detalhes do ambiente](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). O nome da região exibido no Cloud Manager pode ser [mapeado para o código de região](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) usado na API do Cloud Manager.

   __solicitação HTTP de listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. Habilite a saída de porta flexível para um Programa Cloud Manager usando a operação [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager. Use o código `region` apropriado obtido da operação `listRegions` da API do Cloud Manager.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Aguarde 15 minutos para que o programa Cloud Manager provisione a infraestrutura de rede.

3. Verifique se o ambiente concluiu a configuração de __saída de porta flexível__ usando a operação [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) da API do Cloud Manager, usando a `id` retornada da solicitação HTTP `createNetworkInfrastructure` na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém um __status__ de __ready__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

Com a saída de porta flexível criada, agora é possível configurar as regras de encaminhamento de porta usando as APIs do Cloud Manager, conforme descrito abaixo.

>[!ENDTABS]

## Configuração de proxies de saída de porta flexíveis por ambiente

1. Habilite e configure a configuração de __saída de porta flexível__ em cada ambiente do AEM as a Cloud Service usando a operação [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager.

   __Solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Defina os parâmetros JSON em um `flexible-port-egress.json` e fornecido para curl via `... -d @./flexible-port-egress.json`.

   [Baixe o exemplo lex-port-egress.json](./assets/flexible-port-egress.json). Este arquivo é apenas um exemplo. Configure seu arquivo conforme necessário com base nos campos opcionais/obrigatórios documentados em [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   Para cada mapeamento `portForwards`, a rede avançada define a seguinte regra de encaminhamento:

   | Host do proxy | Porta do proxy |  | Host externo | Porta externa |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se sua implantação do AEM __only__ exigir conexões HTTP/HTTPS (porta 80/443) para um serviço externo, deixe a matriz `portForwards` vazia, pois essas regras são necessárias somente para solicitações não HTTP/HTTPS.

1. Para cada ambiente, valide se as regras de saída estão em vigor usando a operação [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager.

   __Solicitação HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Configurações flexíveis de saída de porta podem ser atualizadas usando a operação [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager. Lembre-se de que `enableEnvironmentAdvancedNetworkingConfiguration` é uma operação `PUT`, portanto todas as regras devem ser fornecidas com cada invocação desta operação.

1. Agora, você pode usar a configuração flexível de saída de porta no código e configuração personalizados do AEM.


## Conexão com serviços externos por meio de saída de porta flexível

Com o proxy de saída de porta flexível ativado, o código e a configuração do AEM podem usá-los para fazer chamadas para serviços externos. Há duas opções de chamadas externas que o AEM trata de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos em portas fora do padrão
   + Inclui chamadas HTTP/HTTPS feitas para serviços executados em portas diferentes das portas padrão 80 ou 443.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

As solicitações HTTP/HTTPS do AEM em portas padrão (80/443) são permitidas por padrão e não precisam de configuração ou considerações adicionais.


### HTTP/HTTPS em portas fora do padrão

Ao criar conexões HTTP/HTTPS com portas fora do padrão (não-80/443) do AEM, as conexões devem ser feitas por meio de hosts e portas especiais, fornecidas por meio de espaços reservados.

O AEM fornece dois conjuntos de variáveis especiais do sistema Java™ que mapeiam para proxies HTTP/HTTPS da AEM.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy para conexões HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | Porta proxy para conexões HTTPS (definir fallback para `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | Porta proxy para conexões HTTPS (definir fallback para `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Ao fazer chamadas HTTP/HTTPS para serviços externos em portas fora do padrão, nenhum `portForwards` correspondente deve ser definido usando a operação `enableEnvironmentAdvancedNetworkingConfiguration` da API do Cloud Manager, já que as &quot;regras&quot; de encaminhamento de porta são definidas &quot;no código&quot;.

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
        Exemplo de código Java™ fazendo conexão HTTP/HTTPS do AEM as a Cloud Service com um serviço externo em portas HTTP/HTTPS não padrão.
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


As conexões com serviços externos são então chamadas por meio do `AEM_PROXY_HOST` e da porta mapeada (`portForwards.portOrig`), que o AEM encaminha para o nome de host externo mapeado (`portForwards.name`) e a porta (`portForwards.portDest`).

| Host do proxy | Porta do proxy |  | Host externo | Porta externa |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexão SQL usando JDBC DataSourcePool</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos configurando o pool de fontes de dados JDBC da AEM.
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
      <div><strong><a href="./examples/email-service.md">Serviço de email</a></strong></div>
      <p>
        Exemplo de configuração OSGi usando o AEM para conectar-se a serviços de email externos.
      </p>
    </td>   
</tr></table>
