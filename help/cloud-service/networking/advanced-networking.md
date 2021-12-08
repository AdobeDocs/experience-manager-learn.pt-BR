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
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# Rede avançada

AEM as a Cloud Service oferece três opções para gerenciar a conectividade com serviços externos. Um programa do Cloud Manager e seus AEM ambientes as a Cloud Service só podem usar um único tipo de configuração avançada de rede por vez, para garantir que o tipo mais apropriado seja selecionado.

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
