---
title: Endereço IP de saída dedicado
description: Saiba como configurar e usar o endereço IP de saída dedicado, que permite que as conexões de saída do AEM se originem de um IP dedicado.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 5%

---

# Endereço IP de saída dedicado

Saiba como configurar e usar o endereço IP de saída dedicado, que permite que as conexões de saída do AEM se originem de um IP dedicado.

## O que é o endereço IP de saída dedicado?

Endereço IP de saída dedicado permite que solicitações do AEM as a Cloud Service usem um endereço IP dedicado, permitindo que os serviços externos filtrem solicitações recebidas por esse endereço IP. Curtir [portas de saída flexíveis](./flexible-port-egress.md), o IP de saída dedicado permite que você saia em portas fora do padrão.

Um programa do Cloud Manager só pode ter um __solteiro__ tipo de infraestrutura de rede. Certifique-se de que o endereço IP de saída dedicado seja o mais [tipo adequado de infraestrutura de rede](./advanced-networking.md)  para o AEM as a Cloud Service antes de executar os seguintes comandos.

>[!MORELIKETHIS]
>
> Leia o as a Cloud Service do AEM [documentação avançada de configuração de rede](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) para obter mais detalhes sobre endereço IP de saída dedicado.

## Pré-requisitos

Os seguintes itens são necessários ao configurar o endereço IP de saída dedicado:

+ API do Cloud Manager com [Permissões do proprietário da empresa no Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acesso a [Credenciais de autenticação da API do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como Chave de API)
   + Token de acesso (também conhecido como Token do portador)
+ A ID do programa do Cloud Manager
+ As IDs de ambiente do Cloud Manager

Para obter mais detalhes, assista à seguinte apresentação de como configurar e obter credenciais da API do Cloud Manager e como usá-las para fazer uma chamada de API do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. Os dados fornecidos `curl` assumem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o `\` caractere de quebra de linha com `^`.

## Habilitar endereço IP de saída dedicado no programa

Comece habilitando e configurando o endereço IP de saída dedicado no AEM as a Cloud Service.

1. Primeiro, determine a região em que a Rede avançada é necessária, usando a API do Cloud Manager [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. A variável `region name` O é necessário para fazer chamadas de API subsequentes do Cloud Manager. Normalmente, a região em que o ambiente de Produção reside é usada.

   Encontre a região do seu ambiente as a Cloud Service AEM em [Cloud Manager](https://my.cloudmanager.adobe.com) no [detalhes do ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). O nome da região exibido no Cloud Manager pode ser [mapeado para o código de região](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) usada na API do Cloud Manager.

   __solicitação HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Ative o endereço IP de saída dedicado para um programa do Cloud Manager usando a API do Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. Use o `region` código obtido da API do Cloud Manager `listRegions` operação.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Aguarde 15 minutos para que o programa do Cloud Manager provisione a infraestrutura de rede.

1. Verifique se o programa foi concluído __endereço IP de saída dedicado__ configuração usando a API do Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) operação, utilizando o `id` retornado da solicitação HTTP createNetworkInfrastructure na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém um __status__ de __pronto__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

## Configurar proxies dedicados de endereço IP de saída por ambiente

1. Configure o __endereço IP de saída dedicado__ configuração em cada ambiente do AEM as a Cloud Service usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação.

   __solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Defina os parâmetros JSON em uma `dedicated-egress-ip-address.json` e fornecido para curl via `... -d @./dedicated-egress-ip-address.json`.

   [Baixe o exemplo dedicated-egress-ip-address.json](./assets/dedicated-egress-ip-address.json). Este arquivo é apenas um exemplo. Configure seu arquivo conforme necessário com base nos campos opcionais/obrigatórios documentados em [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   A assinatura HTTP da configuração de endereço IP de saída dedicado é diferente apenas da [porta de saída flexível](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) na medida em que também apoia a `nonProxyHosts` configuração.

   `nonProxyHosts` declara um conjunto de hosts para o qual a porta 80 ou 443 deve ser roteada por meio dos intervalos de endereços IP compartilhados padrão em vez do IP de saída dedicado. `nonProxyHosts` pode ser útil, pois a criação de tráfego por meio de IPs compartilhados pode ser otimizada ainda mais automaticamente pelo Adobe.

   Para cada `portForwards` de rede avançada define a seguinte regra de encaminhamento:

   | Host do proxy | Porta do proxy |  | Host externo | Porta externa |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Para cada ambiente, valide se as regras de saída estão em vigor usando a API do Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação.

   __solicitação HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Configurações de endereço IP de saída dedicado podem ser atualizadas usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. Lembrar `enableEnvironmentAdvancedNetworkingConfiguration` é um `PUT` para que todas as regras sejam fornecidas com cada chamada desta operação.

1. Obtenha o __endereço IP de saída dedicado__ usando um Resolvedor de DNS (como [DNSChecker.org](https://dnschecker.org/)) no host: `p{programId}.external.adobeaemcloud.com`ou executando `dig` na linha de comando.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   O nome de host não pode ser `pinged`, uma vez que se trata de uma saída e _não_ e entrada.

   Observe que __endereço IP de saída dedicado__ é compartilhado por todos os ambientes as a Cloud Service AEM no programa.

1. Agora você pode usar o endereço IP de saída dedicado no código e configuração personalizados do AEM. Geralmente, ao usar o endereço IP de saída dedicado, os serviços externos aos quais o AEM as a Cloud Service se conecta são configurados para permitir apenas o tráfego desse endereço IP dedicado.

## Conexão com serviços externos por meio do endereço IP de saída dedicado

Com o endereço IP de saída dedicado ativado, o código e a configuração do AEM podem usar o IP de saída dedicado para fazer chamadas para serviços externos. Há duas opções de chamadas externas que o AEM trata de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos
   + Inclui chamadas HTTP/HTTPS feitas para serviços executados em portas diferentes das portas padrão 80 ou 443.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

As solicitações HTTP/HTTPS do AEM em portas padrão (80/443) são permitidas por padrão, mas não usarão o endereço IP de saída dedicado se não estiver configurado adequadamente, conforme descrito abaixo.

>[!TIP]
>
> Consulte a documentação dedicada de endereço IP de saída do AEM as a Cloud Service para [o conjunto completo de regras de roteamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Ao criar conexões HTTP/HTTPS do AEM, ao usar o endereço IP de saída dedicado, as conexões HTTP/HTTPS são automaticamente enviadas por proxy do AEM usando o endereço IP de saída dedicado. Nenhum código ou configuração adicional é necessário para oferecer suporte a conexões HTTP/HTTPS.

#### Exemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Exemplo de código Java™ que faz a conexão HTTP/HTTPS do AEM as a Cloud Service para um serviço externo usando o protocolo HTTP/HTTPS.
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
| `AEM_PROXY_HOST` | Host proxy para conexões não HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


As conexões com serviços externos são então chamadas por meio do `AEM_PROXY_HOST` e a porta mapeada (`portForwards.portOrig`), que o AEM roteia para o nome de host externo mapeado (`portForwards.name`) e porta (`portForwards.portDest`).

| Host do proxy | Porta do proxy |  | Host externo | Porta externa |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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
