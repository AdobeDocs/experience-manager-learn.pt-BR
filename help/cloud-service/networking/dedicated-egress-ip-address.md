---
title: Endereço IP de saída exclusivo
description: Saiba como configurar e usar o endereço IP de saída dedicado, que permite que as conexões de saída do AEM sejam originárias de IP dedicado.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
last-substantial-update: 2024-04-26T00:00:00Z
duration: 891
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1360'
ht-degree: 1%

---

# Endereço IP de saída exclusivo

Saiba como configurar e usar o endereço IP de saída dedicado, que permite que as conexões de saída do AEM sejam originárias de IP dedicado.

## O que é o endereço IP de saída dedicado?

O endereço IP de saída dedicado permite que as solicitações do AEM as a Cloud Service usem um endereço IP dedicado, permitindo que os serviços externos filtrem as solicitações recebidas por esse endereço IP. Assim como as [portas de saída flexíveis](./flexible-port-egress.md), o IP de saída dedicado permite que você saia em portas não padrão.

Um Programa Cloud Manager só pode ter um tipo de infraestrutura de rede __único__. Certifique-se de que o endereço IP de saída dedicado seja o [tipo mais apropriado de infraestrutura de rede](./advanced-networking.md) para o AEM as a Cloud Service antes de executar os comandos a seguir.

>[!MORELIKETHIS]
>
> Leia a [documentação avançada de configuração de rede](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) da AEM as a Cloud Service para obter mais detalhes sobre o endereço IP de saída dedicado.

## Pré-requisitos

Os seguintes itens são necessários ao configurar um endereço IP de saída dedicado usando APIs do Cloud Manager:

+ API do Cloud Manager com [permissões de Proprietário da empresa no Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acesso a [credenciais de autenticação da API do Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + ID da organização (também conhecida como ID da organização IMS)
   + ID do cliente (também conhecida como Chave de API)
   + Token de acesso (também conhecido como Token do portador)
+ A ID do programa Cloud Manager
+ As IDs de ambiente do Cloud Manager

Para obter mais detalhes [revise como instalar, configurar e obter credenciais da API do Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth), para usá-las para fazer uma chamada de API do Cloud Manager.

Este tutorial usa `curl` para fazer as configurações da API do Cloud Manager. Os comandos `curl` fornecidos pressupõem uma sintaxe Linux/macOS. Se estiver usando o prompt de comando do Windows, substitua o caractere de quebra de linha `\` por `^`.

## Habilitar endereço IP de saída dedicado no programa

Comece habilitando e configurando o endereço IP de saída dedicado no AEM as a Cloud Service.

>[!BEGINTABS]

>[!TAB Cloud Manager]

O endereço IP de saída dedicado pode ser ativado usando o Cloud Manager. As etapas a seguir descrevem como ativar o endereço IP de saída dedicado no AEM as a Cloud Service usando o Cloud Manager.

1. Faça logon no [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) como Proprietário da empresa do Cloud Manager.
1. Navegue até o Programa desejado.
1. No menu esquerdo, navegue até __Serviços > Infraestrutura de Rede__.
1. Selecione o botão __Adicionar infraestrutura de rede__.

   ![Adicionar infraestrutura de rede](./assets/cloud-manager__add-network-infrastructure.png)

1. Na caixa de diálogo __Adicionar infraestrutura de rede__, selecione a opção __Endereço IP de saída dedicado__ e selecione a __Região__ para criar o endereço IP de saída dedicado.

   ![Adicionar endereço IP de saída dedicado](./assets/dedicated-egress-ip-address/select-type.png)

1. Selecione __Salvar__ para confirmar a adição do endereço IP de saída dedicado.

   ![Confirmar criação de endereço IP de saída dedicado](./assets/dedicated-egress-ip-address/confirmation.png)

1. Aguarde a infraestrutura de rede ser criada e marcada como __Pronta__. Esse processo pode levar até 1 hora.

   ![Status de criação do endereço IP de saída dedicado](./assets/dedicated-egress-ip-address/ready.png)

Com o endereço IP de saída dedicado criado, agora é possível configurá-lo usando as APIs do Cloud Manager, conforme descrito abaixo.

>[!TAB APIs do Cloud Manager]

O endereço IP de saída dedicado pode ser ativado usando as APIs do Cloud Manager. As etapas a seguir descrevem como ativar o endereço IP de saída dedicado no AEM as a Cloud Service usando a API do Cloud Manager.


1. Primeiro, determine a região na qual a Rede Avançada é necessária, usando a operação [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager. O `region name` é necessário para fazer chamadas de API do Cloud Manager subsequentes. Normalmente, a região em que o ambiente de Produção reside é usada.

   Encontre a região do seu ambiente do AEM as a Cloud Service em [Cloud Manager](https://my.cloudmanager.adobe.com) nos [detalhes do ambiente](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). O nome da região exibido no Cloud Manager pode ser [mapeado para o código de região](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) usado na API do Cloud Manager.

   __solicitação HTTP de listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. Habilite o endereço IP de saída dedicado para um Programa Cloud Manager usando a operação [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager. Use o código `region` apropriado obtido da operação `listRegions` da API do Cloud Manager.

   __solicitação HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Aguarde 15 minutos para que o programa Cloud Manager provisione a infraestrutura de rede.

3. Verifique se o programa concluiu a configuração do __endereço IP de saída dedicado__ usando a operação [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) da API do Cloud Manager, usando o `id` retornado da solicitação HTTP `createNetworkInfrastructure` na etapa anterior.

   __solicitação HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Verifique se a resposta HTTP contém um __status__ de __ready__. Se ainda não estiver pronto, verifique novamente o status a cada poucos minutos.

Com o endereço IP de saída dedicado criado, agora é possível configurá-lo usando as APIs do Cloud Manager, conforme descrito abaixo.

>[!ENDTABS]


## Configurar proxies dedicados de endereço IP de saída por ambiente

1. Configure a configuração do __endereço IP de saída dedicado__ em cada ambiente do AEM as a Cloud Service usando a operação [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager.

   __Solicitação HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Defina os parâmetros JSON em um `dedicated-egress-ip-address.json` e fornecido para curl via `... -d @./dedicated-egress-ip-address.json`.

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

   A assinatura HTTP da configuração de endereço IP de saída dedicado difere apenas da [porta de saída flexível](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment), pois ela também oferece suporte à configuração `nonProxyHosts` opcional.

   `nonProxyHosts` declara um conjunto de hosts para o qual a porta 80 ou 443 deve ser roteada através dos intervalos de endereços IP compartilhados padrão em vez do IP de saída dedicado. `nonProxyHosts` pode ser útil, pois a geração de tráfego por meio de IPs compartilhados é otimizada automaticamente pela Adobe.

   Para cada mapeamento `portForwards`, a rede avançada define a seguinte regra de encaminhamento:

   | Host do proxy | Porta do proxy |  | Host externo | Porta externa |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Para cada ambiente, valide se as regras de saída estão em vigor usando a operação [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager.

   __Solicitação HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Configurações de endereço IP de saída dedicado podem ser atualizadas usando a operação [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) da API do Cloud Manager. Lembre-se de que `enableEnvironmentAdvancedNetworkingConfiguration` é uma operação `PUT`, portanto todas as regras devem ser fornecidas com cada invocação desta operação.

1. Obtenha o __endereço IP de saída dedicado__ usando um Resolvedor de DNS (como [DNSChecker.org](https://dnschecker.org/)) no host: `p{programId}.external.adobeaemcloud.com` ou executando `dig` na linha de comando.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   O nome de host não pode ser `pinged`, pois é uma saída e _não_ e entrada.

   Observe que o endereço IP de saída dedicado é compartilhado por todos os ambientes AEM as a Cloud Service no programa.

1. Agora, você pode usar o endereço IP de saída dedicado no código e configuração personalizados do AEM. Geralmente, ao usar o endereço IP de saída dedicado, os serviços externos aos quais o AEM as a Cloud Service se conecta são configurados para permitir apenas o tráfego desse endereço IP dedicado.

## Conexão com serviços externos por meio do endereço IP de saída dedicado

Com o endereço IP de saída dedicado ativado, o código e a configuração do AEM podem usar o IP de saída dedicado para fazer chamadas para serviços externos. Há duas opções de chamadas externas que o AEM trata de forma diferente:

1. Chamadas HTTP/HTTPS para serviços externos
   + Inclui chamadas HTTP/HTTPS feitas para serviços executados em portas diferentes das portas padrão 80 ou 443.
1. chamadas não HTTP/HTTPS para serviços externos
   + Inclui chamadas não HTTP, como conexões com servidores de email, bancos de dados SQL ou serviços que são executados em outros protocolos não HTTP/HTTPS.

As solicitações HTTP/HTTPS do AEM em portas padrão (80/443) são permitidas por padrão, mas não usam o endereço IP de saída dedicado se não estiverem configuradas adequadamente conforme descrito abaixo.

>[!TIP]
>
> Consulte a documentação de endereço IP de saída dedicado da AEM as a Cloud Service para [o conjunto completo de regras de roteamento](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).


### HTTP/HTTPS

Ao criar conexões HTTP/HTTPS do AEM, ao usar o endereço IP de saída dedicado, as conexões HTTP/HTTPS são automaticamente enviadas por proxy da AEM usando o endereço IP de saída dedicado. Nenhum código ou configuração adicional é necessário para oferecer suporte a conexões HTTP/HTTPS.

#### Exemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Exemplo de código Java™ fazendo conexão HTTP/HTTPS do AEM as a Cloud Service com um serviço externo usando o protocolo HTTP/HTTPS.
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


As conexões com serviços externos são então chamadas por meio do `AEM_PROXY_HOST` e da porta mapeada (`portForwards.portOrig`), que o AEM encaminha para o nome de host externo mapeado (`portForwards.name`) e a porta (`portForwards.portDest`).

| Host do proxy | Porta do proxy |  | Host externo | Porta externa |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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
