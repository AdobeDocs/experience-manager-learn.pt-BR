---
title: Noções básicas da autenticação do Adobe IMS com o AEM no Adobe Managed Services
description: O Adobe Experience Manager apresenta o suporte do Admin Console para instâncias do AEM e a autenticação baseada no Adobe IMS (Identity Management System) para o AEM no Managed Services.   Essa integração permite que os clientes do AEM Managed Services gerenciem todos os usuários da Experience Cloud em um único console da Web unificado. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias do AEM, concedendo acesso gerenciado centralmente a instâncias específicas do AEM.
version: 6.4, 6.5
feature: 'Usuários e grupos '
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Arquitetura
role: Arquiteto
level: Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---


# Noções básicas sobre a autenticação do Adobe IMS com o AEM no Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

O Adobe Experience Manager apresenta o suporte do Admin Console para instâncias do AEM e a autenticação baseada no Adobe IMS (Identity Management System) para o AEM no Managed Services.   Essa integração permite que os clientes do AEM Managed Services gerenciem todos os usuários da Experience Cloud em um único console da Web unificado. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias do AEM, concedendo acesso gerenciado centralmente a instâncias específicas do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* O suporte de autenticação IMS do Adobe Experience Manager é somente para usuários &quot;internos&quot; (autores, revisores, administradores, desenvolvedores, etc.) e não para usuários finais externos, como visitantes do site.
* [O Admin ](https://adminconsole.adobe.com/) Console representará clientes do AEM Managed Services como Orgs do IMS e as instâncias do AEM como Contextos de produto. O sistema do Admin Console e os administradores de produtos podem definir e gerenciar.
* Os AEM Managed Services sincronizam sua topologia com o Admin Console, criando um mapeamento de 1 para 1 entre um Contexto de produto e uma instância do AEM.
* O Perfil do produto no Admin Console determinará quais instâncias do AEM um usuário pode acessar.
* O suporte de autenticação inclui IDPs compatíveis com SAML2 do cliente para SSO.
* Somente as Enterprise ID ou Federated ID (para SSO do cliente) serão compatíveis (as IDs pessoais da Adobe não são compatíveis).

** Esse recurso é compatível com o AEM 6.4 SP3 e posterior para clientes do Adobe Managed Services.*

## Práticas recomendadas {#best-practices}

### Aplicação de permissões no Admin Console

A aplicação de permissões e acesso no nível do usuário deve ser evitada no Admin Console e no Adobe Experience Manager.

No Admin Console, os usuários devem receber acesso por meio de Grupos de usuários no nível Contexto do Produto. Em geral, os grupos de usuários são melhor expressos por função lógica na organização para promover a reutilização dos grupos em produtos da Adobe Experience Cloud.

>[!NOTE]
>
> Se estiver usando o AEM as a Cloud Service, atribua usuários do Admin Console diretamente a Perfis de produto. Não há suporte para permissões transitórias entre usuários do Admin Console para perfis de produto por meio de grupos de usuários do Admin Console no AEM as a Cloud Service.

### Aplicação de permissões no Adobe Experience Manager

No Adobe Experience Manager, os grupos de usuários sincronizados do Adobe IMS devem ser adicionados por termo a [grupos de usuários fornecidos pelo AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vêm pré-configurados com as permissões apropriadas para executar conjuntos específicos de tarefas no AEM. Os usuários sincronizados do Adobe IMS não devem ser adicionados diretamente a [grupos de usuários fornecidos pelo AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
