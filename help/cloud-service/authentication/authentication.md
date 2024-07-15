---
title: Autenticação no AEM as a Cloud Service
description: Saiba mais sobre autenticação no AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 3%

---

# Autenticação AEM as a Cloud Service

O AEM as a Cloud Service oferece suporte a várias opções de autenticação e varia de acordo com o tipo de serviço.

|                       | Autor do AEM | Publicação no AEM |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| · [SAML 2.0 via Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Logon único (SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [Autenticação do token](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |
| Autenticação básica | ✘ | ✘ |

## Opções de autenticação

Clique no link correspondente abaixo para obter detalhes sobre como configurar e usar a abordagem de autenticação.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Gerencie o acesso do autor do AEM usando o Adobe IMS pela Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Autentique o usuário do site em um IDP usando a integração SAML 2.0 do serviço AEM Publish.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Autenticação do token</a></strong></div>
      <p>
        Permitir que aplicativos e middleware se autentiquem no AEM usando um token de serviço de API.
      </p>
    </td>   
  </tr>
</table>
