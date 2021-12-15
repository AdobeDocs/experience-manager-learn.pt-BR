---
title: VPN (Virtual Private Network)
description: Saiba como se conectar AEM as a Cloud Service com sua VPN para criar canais de comunicação seguros entre AEM e serviços internos.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: ba2c299baeda632d6ebeff0c6ee07de5ef29b9cb
workflow-type: tm+mt
source-wordcount: '1259'
ht-degree: 0%

---

# VPN (Virtual Private Network)

Saiba como se conectar AEM as a Cloud Service com sua VPN para criar canais de comunicação seguros entre AEM e serviços internos.

## O que é a rede privada virtual?

A VPN (Virtual Private Network) permite que um cliente AEM as a Cloud Service conecte um programa do Cloud Manager a um já existente, [compatível](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) VPN. Isso permite conexões seguras e controladas entre AEM serviços as a Cloud Service e na rede do cliente.

Um programa do Cloud Manager só pode ter um __individual__ tipo de infraestrutura de rede. Certifique-se de que a rede privada virtual seja a mais [tipo adequado de infraestrutura de rede](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#general-vpn-considerations) para seu AEM as a Cloud Service antes de executar os seguintes comandos.

>[!MORELIKETHIS]
>
> Leia o AEM as a Cloud Service [documentação de configuração de rede avançada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) para obter mais detalhes sobre a Virtual Private Network.

## Pré-requisitos

Os itens a seguir são necessários ao configurar a Rede Virtual Privada:

+ conta Adobe com [Permissões de Proprietário comercial do Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Acesso ao [Credenciais de autenticação da API do Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como chave da API)
   + Token de acesso (também conhecido como Token portador)
+ A ID do programa do Cloud Manager
+ As IDs do ambiente do Cloud Manager
+ Uma rede privada virtual, com acesso a todos os parâmetros de conexão necessários.

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. O `curl` Os comandos assumem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o `\` caractere de quebra de linha com `^`.

## Habilitar Rede Privada Virtual por programa

Comece ativando a Rede privada virtual AEM as a Cloud Service.

1. Primeiro, determine a região em que a Rede avançada será configurada usando a API do Cloud Manager. [listRegiões](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) operação. O `region name` será necessário fazer chamadas de API subsequentes do Cloud Manager. Normalmente, a região em que o ambiente de Produção reside é usada.

   __solicitação HTTP listRegiões__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Habilitar rede privada virtual para um programa do Cloud Manager usando APIs do Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) operação. Use o `region` código obtido da API do Cloud Manager `listRegions` operação.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Defina os parâmetros JSON em um `vpn-create.json` e fornecidos por via de curl `... -d @./vpn-create.json`.

[Baixe o exemplo vpn-create.json](./assets/vpn-create.json)

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Aguarde de 45 a 60 minutos para que o Programa Cloud Manager forneça a infraestrutura de rede.

1. Verifique se o ambiente foi concluído __Rede privada virtual__ configuração usando a API do Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) , usando a `id` retornado da solicitação HTTP createNetworkInfrastructure na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém uma __status__ de __ready__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

## Configurar proxies de rede privada virtual por ambiente

1. Ative e configure a variável __Rede privada virtual__ configuração em cada ambiente AEM as a Cloud Service usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operação.

   __solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Defina os parâmetros JSON em um `vpn-configure.json` e fornecidos por via de curl `... -d @./vpn-configure.json`.

[Baixe o exemplo vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
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

   `nonProxyHosts` declara um conjunto de hosts para os quais a porta 80 ou 443 deve ser roteada por meio dos intervalos de endereço IP compartilhado padrão em vez do IP de saída dedicado. Isso pode ser útil, pois a criação de tráfego por meio de IPs compartilhados pode ser otimizada ainda mais automaticamente pelo Adobe.

   Para cada `portForwards` , a rede avançada define a seguinte regra de encaminhamento:

   | Host proxy | Porta proxy |  | Host externo | Porta externa |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Se sua implantação de AEM __only__ requer conexões HTTP/HTTPS para serviço externo, deixe a variável `portForwards` matriz vazia, pois essas regras são necessárias apenas para solicitações não HTTP/HTTPS.


1. Para cada ambiente, valide se as regras de roteamento de vpn estão em vigor usando a API do Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) operação.

   __solicitação HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. As configurações de proxy de rede privada virtual podem ser atualizadas usando a API do Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operação. Lembrar `enableEnvironmentAdvancedNetworkingConfiguration` é um `PUT` , portanto, todas as regras devem ser fornecidas com cada invocação desta operação.

1. Agora você pode usar a configuração de saída da Rede privada virtual em seu código e configuração de AEM personalizados.

## Conexão com serviços externos pela rede privada virtual

Com a Rede privada virtual ativada, AEM código e configuração podem usá-las para fazer chamadas para serviços externos por meio da VPN. Há dois sabores de chamadas externas que AEM tratam de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos em portas não padrão
   + Inclui chamadas HTTP/HTTPS feitas para serviços em execução em portas diferentes das portas 80 ou 443 padrão.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

Por padrão, as solicitações HTTP/HTTPS de AEM em portas padrão (80/443) são permitidas e não precisam de configuração ou considerações adicionais.

### HTTP/HTTPS em portas não padrão

Ao criar conexões HTTP/HTTPS com portas não padrão (não-80/443) a partir de AEM, a conexão deve ser feita por meio de host e portas especiais, fornecidas por meio de espaços reservados.

AEM fornece dois conjuntos de variáveis especiais do sistema Java™ que mapeiam para AEM proxies HTTP/HTTPS.

| Nome da variável | Utilização | Código Java™ | Configuração OSGi | Configuração mod_proxy do servidor Web Apache | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Host proxy para conexões HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Porta proxy para conexões HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Host proxy para conexões HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Porta proxy para conexões HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

As solicitações para serviços externos HTTP/HTTPS devem ser feitas configurando a configuração de proxy do cliente HTTP do Java™ por meio de valores de hosts/portas de proxy AEM.

Ao fazer chamadas HTTP/HTTPS para serviços externos em portas não padrão, nenhum `portForwards` deve ser definido usando as APIs do Cloud Manager `__enableEnvironmentAdvancedNetworkingConfiguration` , já que as &quot;regras&quot; de encaminhamento de porta são definidas como &quot;em código&quot;.

>[!TIP]
>
> Consulte AEM documentação da Virtual Private Network do as a Cloud Service para obter [o conjunto completo de regras de roteamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Exemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS em portas não padrão" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS em portas não padrão</a></strong></div>
    <p>
        Exemplo de código Java™ tornando a conexão HTTP/HTTPS de AEM as a Cloud Service para um serviço externo em portas HTTP/HTTPS não padrão.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Exemplos de código de conexões não HTTP/HTTPS

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

### Limitar acesso a AEM as a Cloud Service via VPN

A configuração da Rede Privada Virtual permite que o acesso a AEM ambientes as a Cloud Service seja limitado ao acesso VPN.

#### Exemplos de configuração

<table><tr>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en"><img alt="Aplicação de uma lista de permissões IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en">Aplicação de uma  de lista de permissões IP</a></strong></div>
      <p>
            Configure uma  IP lista de permissões de forma que somente o tráfego VPN possa acessar AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Restrições de acesso à VPN baseada em caminho para o AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Restrições de acesso à VPN baseada em caminho para o AEM Publish</a></strong></div>
      <p>
            Exigir acesso VPN para caminhos específicos na publicação do AEM.
      </p>
    </td>
   <td></td>
</tr></table>
