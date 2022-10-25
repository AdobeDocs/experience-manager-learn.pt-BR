---
title: Autenticação em AEM as a Cloud Service
description: Saiba mais sobre autenticação no AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 4%

---

# AEM autenticação as a Cloud Service

AEM as a Cloud Service suporta várias opções de autenticação e varia de acordo com o tipo de serviço.

|  | Autor do AEM | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Autenticação de token](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## Opções de autenticação

Clique no link correspondente abaixo para obter detalhes sobre como configurar e usar a abordagem de autenticação.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Gerencie o acesso do autor do AEM usando o Adobe IMS por meio da Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Autentique o usuário de seu site para um IDP usando a integração SAML 2.0 do serviço de publicação do AEM.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Autenticação de token</a></strong></div>
      <p>
        Permita que aplicativos e middleware sejam autenticados em AEM usando um token de serviço de API.
      </p>
    </td>   
  </tr>
</table>
