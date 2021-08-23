---
title: Noções básicas sobre a autenticação Adobe IMS com o AEM no Adobe Managed Services
description: A Adobe Experience Manager apresenta suporte Admin Console para instâncias de AEM e autenticação Adobe IMS (Identity Management System) para AEM no Managed Services.   Essa integração permite que AEM clientes do Managed Services gerenciem todos os usuários do Experience Cloud em um único console da Web unificado. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias de AEM, concedendo acesso gerenciado centralmente às instâncias de AEM específicas.
version: 6.4, 6.5
feature: Usuários e grupos
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Arquitetura
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---


# Noções básicas sobre a autenticação Adobe IMS com o AEM no Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

A Adobe Experience Manager apresenta suporte Admin Console para instâncias de AEM e autenticação Adobe IMS (Identity Management System) para AEM no Managed Services.   Essa integração permite que AEM clientes do Managed Services gerenciem todos os usuários do Experience Cloud em um único console da Web unificado. Os usuários e grupos podem ser atribuídos a perfis de produto associados a instâncias de AEM, concedendo acesso gerenciado centralmente às instâncias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* O suporte de autenticação do Adobe Experience Manager IMS é somente para usuários &quot;internos&quot; (autores, revisores, administradores, desenvolvedores, etc.) e não para usuários finais externos, como visitantes do site.
* [O Admin ](https://adminconsole.adobe.com/) Console representará AEM clientes do Managed Services como Orgs do IMS e as instâncias AEM como Contextos do produto. Os Admin Console System e Product Admins podem definir e gerenciar.
* AEM Managed Services sincronize sua topologia com o Admin Console, criando um mapeamento de 1 para 1 entre um Contexto de produto e uma instância AEM.
* O Perfil do produto no Admin Console determinará quais instâncias de AEM um usuário pode acessar.
* O suporte de autenticação inclui IDPs compatíveis com SAML2 do cliente para SSO.
* Somente as Enterprise ou Federated IDs (para SSO do cliente) serão compatíveis (as IDs de Adobe pessoal não são compatíveis).

** Esse recurso é compatível com o AEM 6.4 SP3 e posterior para clientes do Adobe Managed Services.*

## Práticas recomendadas {#best-practices}

### Aplicação de permissões no Admin Console

A aplicação de permissões e acesso no nível do usuário deve ser evitada no Admin Console e no Adobe Experience Manager.

No Admin Console, os usuários devem receber acesso por meio de Grupos de usuários no nível Contexto do Produto. Em geral, os grupos de usuários são melhor expressos por função lógica na organização para promover a reutilização dos grupos em produtos da Adobe Experience Cloud.

>[!NOTE]
>
> Se estiver usando o AEM como um Cloud Service, atribua os usuários do Admin Console diretamente aos Perfis de produto. Permissões transitórias entre usuários do Admin Console para Perfis de produto por meio de grupos de usuários do Admin Console não são suportadas para o AEM as a Cloud Service.

### Aplicação de permissões no Adobe Experience Manager

No Adobe Experience Manager, os grupos de usuários sincronizados do Adobe IMS devem ser adicionados por termo a [grupos de usuários fornecidos pelo AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vêm pré-configurados com as permissões apropriadas para executar conjuntos específicos de tarefas no AEM. Os usuários sincronizados do Adobe IMS não devem ser adicionados diretamente a [grupos de usuários fornecidos pelo AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
