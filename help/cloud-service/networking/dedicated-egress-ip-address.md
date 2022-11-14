---
title: Endereço IP de saída dedicado
description: Saiba como configurar e usar o endereço IP de saída dedicado, que permite que conexões de saída de AEM sejam originadas de um IP dedicado.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: b74dc2693071313a80ccaaea839b8e2087c9edaa
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 4%

---

# Endereço IP de saída dedicado

Saiba como configurar e usar o endereço IP de saída dedicado, que permite que conexões de saída de AEM sejam originadas de um IP dedicado.

## O que é endereço IP de saída dedicado?

O endereço IP de saída dedicado permite que as solicitações AEM as a Cloud Service usem um endereço IP dedicado, permitindo que os serviços externos filtrem as solicitações recebidas por esse endereço IP. Like [portas de saída flexíveis](./flexible-port-egress.md), o IP de saída dedicado permite que você saia em portas não padrão.

Um programa do Cloud Manager só pode ter um __individual__ tipo de infraestrutura de rede. Certifique-se de que o endereço IP de saída dedicado seja o mais [tipo adequado de infraestrutura de rede](./advanced-networking.md)  para seu AEM as a Cloud Service antes de executar os seguintes comandos.

>[!MORELIKETHIS]
>
> Leia o AEM as a Cloud Service [documentação de configuração de rede avançada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) para obter mais detalhes sobre endereço IP de saída dedicado.

## Pré-requisitos

Os itens a seguir são necessários ao configurar o endereço IP de saída dedicado:

+ API do Cloud Manager com [Permissões de Proprietário comercial do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acesso ao [Credenciais de autenticação da API do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como chave da API)
   + Token de acesso (também conhecido como Token portador)
+ A ID do programa do Cloud Manager
+ As IDs do ambiente do Cloud Manager

Para obter mais detalhes, consulte a seguinte apresentação sobre como configurar, configurar e obter credenciais da API do Cloud Manager e como usá-las para fazer uma chamada da API do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. O `curl` Os comandos assumem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o `\` caractere de quebra de linha com `^`.

## Habilitar endereço IP de saída dedicado no programa

Comece ativando e configurando o endereço IP de saída dedicado AEM as a Cloud Service.

1. Primeiro, determine a região na qual a Rede avançada é necessária usando a API do Cloud Manager. [listRegiões](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. O `region name` O é necessário para fazer chamadas de API subsequentes do Cloud Manager. Normalmente, a região em que o ambiente de Produção reside é usada.

   __solicitação HTTP listRegiões__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Habilitar endereço IP de saída dedicado para um programa do Cloud Manager usando a API do Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. Use o `region` código obtido da API do Cloud Manager `listRegions` operação.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Aguarde 15 minutos para que o Programa Cloud Manager forneça a infraestrutura de rede.

1. Verifique se o programa foi concluído __endereço IP de saída dedicado__ configuração usando a API do Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) , usando a `id` retornado da solicitação HTTP createNetworkInfrastructure na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém uma __status__ de __ready__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

## Configurar proxies de endereço IP de saída dedicados por ambiente

1. Configure o __endereço IP de saída dedicado__ configuração em cada ambiente AEM as a Cloud Service usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação.

   __solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Defina os parâmetros JSON em um `dedicated-egress-ip-address.json` e fornecidos por via de curl `... -d @./dedicated-egress-ip-address.json`.

   [Baixe o exemplo dedicado-egress-ip-address.json](./assets/dedicated-egress-ip-address.json). Este arquivo é apenas um exemplo. Configure seu arquivo conforme necessário com base nos campos opcionais/obrigatórios documentados em [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   A assinatura HTTP da configuração do endereço IP de saída dedicada só difere de [porta de saída flexível](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) na medida em que também suporta o `nonProxyHosts` configuração.

   `nonProxyHosts` declara um conjunto de hosts para os quais a porta 80 ou 443 deve ser roteada por meio dos intervalos de endereço IP compartilhado padrão em vez do IP de saída dedicado. `nonProxyHosts` pode ser útil, pois o rastreamento de tráfego por meio de IPs compartilhados pode ser otimizado ainda mais automaticamente pelo Adobe.

   Para cada `portForwards` , a rede avançada define a seguinte regra de encaminhamento:

   | Host proxy | Porta proxy |  | Host externo | Porta externa |
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

1. As configurações de endereço IP de saída dedicado podem ser atualizadas usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operação. Lembrar `enableEnvironmentAdvancedNetworkingConfiguration` é um `PUT` , portanto, todas as regras devem ser fornecidas com cada invocação desta operação.

1. Obtenha o __endereço IP de saída dedicado__ usando um DNS Resolver (como [DNSChecker.org](https://dnschecker.org/)) no host: `p{programId}.external.adobeaemcloud.com`ou executando `dig` na linha de comando.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   O nome de host não pode ser `pinged`, uma vez que se trata de um passo e _not_ e ingresso.

   Observe que __endereço IP de saída dedicado__ é compartilhado por todos os ambientes AEM as a Cloud Service do programa.

1. Agora você pode usar o endereço IP de saída dedicado em seu código e configuração de AEM personalizados. Geralmente, ao usar endereço IP de saída dedicado, os serviços externos AEM conectados ao são configurados para permitir apenas o tráfego desse endereço IP dedicado.

## Conexão com serviços externos via endereço IP de saída dedicado

Com o endereço IP de saída dedicado ativado, o código e a configuração AEM podem usar o IP de saída dedicado para fazer chamadas para serviços externos. Há dois sabores de chamadas externas que AEM tratam de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos
   + Inclui chamadas HTTP/HTTPS feitas para serviços em execução em portas diferentes das portas 80 ou 443 padrão.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

Por padrão, as solicitações HTTP/HTTPS de AEM em portas padrão (80/443) são permitidas, mas não usarão o endereço IP de saída dedicado se não estiver configurado adequadamente, conforme descrito abaixo.

>[!TIP]
>
> Consulte AEM documentação de endereço IP de saída dedicada do as a Cloud Service para [o conjunto completo de regras de roteamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Ao criar conexões HTTP/HTTPS a partir do AEM, ao usar endereço IP de saída dedicado, as conexões HTTP/HTTPS são automaticamente enviadas por proxy usando o endereço IP de saída dedicado. Não é necessário nenhum código ou configuração adicional para suportar conexões HTTP/HTTPS.

#### Exemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Exemplo de código Java™ tornando a conexão HTTP/HTTPS do AEM as a Cloud Service para um serviço externo usando o protocolo HTTP/HTTPS.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Conexões não HTTP/HTTPS para serviços externos

Ao criar conexões não HTTP/HTTPS (por exemplo, SQL, SMTP e assim por diante) do AEM, a conexão deve ser feita por meio de um nome de host especial fornecido pelo AEM.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy para conexões não HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


As conexões com serviços externos são então chamadas por meio da variável `AEM_PROXY_HOST` e a porta mapeada (`portForwards.portOrig`), que AEM então roteia para o nome de host externo mapeado (`portForwards.name`) e a porta (`portForwards.portDest`).

| Host proxy | Porta proxy |  | Host externo | Porta externa |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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
