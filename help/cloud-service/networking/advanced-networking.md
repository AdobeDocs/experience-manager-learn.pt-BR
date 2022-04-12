---
title: Rede avançada
description: Saiba mais sobre AEM opções avançadas de rede do as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 3%

---

# Rede avançada

AEM as a Cloud Service fornece recursos avançados de rede que permitem o gerenciamento preciso de conexões com e AEM programas as a Cloud Service.

|  | [Programas de produção](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programas do Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Suporta rede avançada | ✔ | ✘ |


AEM rede avançada é composta por três opções para gerenciar a conectividade com serviços externos. Um programa do Cloud Manager e seus AEM ambientes as a Cloud Service só podem usar um único tipo de configuração avançada de rede por vez, para garantir que o tipo mais apropriado seja selecionado.

|  | HTTP/HTTPS em portas padrão | HTTP/HTTPS em portas não padrão | Conexões não HTTP/HTTPS | IP de saída dedicado | Lista &quot;Sem hosts proxy&quot; | Ligar a serviços protegidos por VPN | Limitar o tráfego de publicação do AEM por IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Sem rede avançada__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Saída flexível da porta__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Endereço IP de saída dedicado__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Rede privada virtual__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Para obter mais detalhes sobre as considerações envolvidas ao selecionar o tipo de rede avançado apropriado, consulte [documentação de rede avançada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutoriais avançados de rede

Depois que a opção de rede avançada mais apropriada baseada na necessidade da sua organização tiver sido identificada, clique no tutorial correspondente abaixo para obter instruções passo a passo e exemplos de código.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Saída flexível da porta" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Saída flexível da porta</a></strong></div>
      <p>
          Permitir AEM tráfego as a Cloud Service de saída em portas não padrão.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Endereço IP de saída dedicado" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Endereço IP de saída dedicado</a></strong></div>
      <p>
        Origine AEM tráfego as a Cloud Service de saída de um IP dedicado.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="VPN (Virtual Private Network)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">VPN (Virtual Private Network)</a></strong></div>
      <p>
        Tráfego seguro entre uma infraestrutura de cliente ou fornecedor e AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Exemplos de código

Esta coleção fornece exemplos da configuração e código necessários para aproveitar os recursos avançados de rede para casos de uso específicos.

Certifique-se de que [configuração avançada de rede](#advanced-networking) O foi configurado antes de seguir esses tutoriais.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="VPN (Virtual Private Network)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Serviço de email</a></strong></div>
      <p>
        Exemplo de configuração do OSGi usando AEM para se conectar a serviços de email externos.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS em portas não padrão" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS em portas não padrão</a></strong></div>
        <p>
            Exemplo de código Java™ tornando a conexão HTTP/HTTPS de AEM as a Cloud Service para um serviço externo em portas HTTP/HTTPS não padrão.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexão SQL usando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexão SQL usando JDBC DataSourcePool</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos configurando AEM pool de fonte de dados JDBC.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexão SQL usando APIs Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexão SQL usando APIs Java™</a></strong></div>
      <p>
            Exemplo de código Java™ conectando-se a bancos de dados SQL externos usando as APIs SQL do Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Aplicação de uma lista de permissões IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Aplicação de uma  de lista de permissões IP</a></strong></div>
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
</tr>
</table>
