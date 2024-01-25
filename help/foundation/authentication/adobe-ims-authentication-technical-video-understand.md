---
title: Noções básicas sobre a autenticação do Adobe IMS com AEM no Adobe Managed Services
description: O Adobe Experience Manager apresenta suporte para o Admin Console em instâncias do AEM e autenticação baseada no Adobe IMS (Identity Management AEM System) para o no Managed Services.   Essa integração permite que os clientes do AEM Managed Services gerenciem todos os usuários do Experience Cloud em um único console unificado da Web. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias de AEM, concedendo acesso gerenciado centralmente às instâncias específicas de AEM.
version: 6.4, 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 420
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Noções básicas sobre a autenticação do Adobe IMS com AEM no Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

O Adobe Experience Manager introduz o suporte ao Admin Console para instâncias do AEM e a autenticação baseada no Adobe Identity Management AEM System (IMS) para o no Managed Services.   Essa integração permite que os clientes do AEM Managed Services gerenciem todos os usuários do Experience Cloud em um único console unificado da Web. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias de AEM, concedendo acesso gerenciado centralmente às instâncias específicas de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* O suporte à autenticação do Adobe Experience Manager IMS é somente para usuários &quot;internos&quot; (autores, revisores, administradores, desenvolvedores e assim por diante) e não para usuários finais externos, como visitantes de sites.
* [Admin Console](https://adminconsole.adobe.com/) O representa clientes do Managed Services com AEM como Organizações IMS e as instâncias AEM como Contextos do produto. Os administradores de sistema e de produto do Admin Console podem definir e gerenciar.
* O AEM Managed Services sincroniza sua topologia com o Admin Console, criando um mapeamento de 1 para 1 entre um Contexto do produto e uma instância do AEM.
* O Perfil de produto no Admin Console determina quais instâncias de AEM um usuário pode acessar.
* O suporte à autenticação inclui IDPs compatíveis com SAML2 do cliente para SSO.
* Somente IDs Enterprise ou Federated (para SSO do cliente) são compatíveis (as IDs de Adobe pessoal não são compatíveis).

*&#42;Esse recurso é compatível com AEM 6.4 SP3 e posterior para clientes do Adobe Managed Services.*

## Práticas recomendadas {#best-practices}

### Aplicação de permissões no Admin Console

A aplicação de permissões e acesso no nível do usuário deve ser evitada no Admin Console e no Adobe Experience Manager.

No, os usuários do Admin Console devem receber acesso por meio dos Grupos de usuários no nível do Contexto do produto. Normalmente, os grupos de usuários são melhor expressos pela função lógica na organização para promover a reutilização dos grupos em todos os produtos da Adobe Experience Cloud.

>[!NOTE]
>
> Se estiver usando o AEM as a Cloud Service, atribua os usuários do Admin Console diretamente aos Perfis de produto. Permissões transitivas entre usuários de Admin Console para Perfis de produto por meio de grupos de usuários de Admin Console não são compatíveis com o AEM as a Cloud Service.

### Aplicação de permissões no Adobe Experience Manager

No Adobe Experience Manager, os grupos de usuários sincronizados do Adobe IMS devem estar no termo adicionado a [Grupos de usuários fornecidos pelo AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html), que vêm pré-configurados com as permissões apropriadas para executar conjuntos específicos de tarefas no AEM. Os usuários sincronizados do Adobe IMS não devem ser adicionados diretamente ao [Grupos de usuários fornecidos pelo AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html).
