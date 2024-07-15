---
title: Rede avançada
description: Saiba mais sobre as opções avançadas de rede da AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 2%

---

# Rede avançada

O AEM as a Cloud Service fornece recursos avançados de rede que permitem o gerenciamento preciso de conexões de e para programas AEM as a Cloud Service.

|                                                   | [Programas de produção](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programas do Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Oferece suporte a redes avançadas | ✔ | ✘ |


A rede avançada da AEM é composta de três opções para gerenciar a conectividade com serviços externos. Um programa Cloud Manager e seus ambientes AEM as a Cloud Service só podem usar um único tipo de configuração avançada de rede por vez, portanto, verifique se o tipo mais apropriado está selecionado.

|                                   | HTTP/HTTPS em portas padrão | HTTP/HTTPS em portas fora do padrão | Conexões não HTTP/HTTPS | IP de saída dedicado | Lista &quot;Hosts sem proxy&quot; | Conectar-se a serviços protegidos por VPN | Limitar o tráfego de Publish do AEM por IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Nenhuma rede avançada__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Saída de porta flexível__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Endereço IP de saída dedicado__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Rede Virtual Privada__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Para obter mais detalhes sobre as considerações envolvidas ao selecionar o tipo de rede avançada apropriado, consulte a [documentação avançada sobre rede](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutoriais avançados de rede

Depois que a opção de rede avançada mais apropriada com base na necessidade de sua organização for identificada, clique no tutorial correspondente abaixo para obter instruções passo a passo e amostras de código.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Saída de porta flexível" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Saída de porta flexível</a></strong></div>
      <p>
          Permitir tráfego de saída do AEM as a Cloud Service em portas fora do padrão.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Endereço IP de saída FileDedicated" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Endereço IP de saída dedicado</a></strong></div>
      <p>
        Originar o tráfego de saída do AEM as a Cloud Service de um IP dedicado.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="VPN (Virtual Private Network)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">VPN (Virtual Private Network)</a></strong></div>
      <p>
        Tráfego seguro entre uma infraestrutura de cliente ou fornecedor e a AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Exemplos de código

Esta coleção fornece exemplos da configuração e do código necessários para aproveitar os recursos avançados de rede para casos de uso específicos.

Verifique se a [configuração avançada de rede](#advanced-networking) apropriada foi definida antes de seguir esses tutoriais.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="VPN (Virtual Private Network)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Serviço de email</a></strong></div>
      <p>
        Exemplo de configuração OSGi usando AEM para se conectar a serviços de email externos.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Exemplo de código Java™ fazendo conexão HTTP/HTTPS do AEM as a Cloud Service com um serviço externo usando o protocolo HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexão SQL usando JDBC DataSourcePool</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos configurando o pool de fontes de dados JDBC do AEM.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexão SQL usando APIs Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexão SQL usando APIs Java™</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos usando APIs SQL do Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Aplicação de uma lista de permissões de IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Aplicando uma inclui na lista de permissões de IP</a></strong></div>
      <p>
            Configure uma inclui na lista de permissões de IP de modo que somente o tráfego de VPN possa acessar o AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Restrições de acesso VPN baseadas em caminho para AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Restrições de acesso à VPN baseada em caminho para AEM Publish</a></strong></div>
      <p>
            Exigir acesso VPN para caminhos específicos no AEM Publish.
      </p>
    </td>
</tr>
</table>
