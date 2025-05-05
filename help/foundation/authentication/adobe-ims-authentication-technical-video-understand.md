---
title: Noções básicas sobre a autenticação do Adobe IMS com o AEM no Adobe Managed Services
description: O Adobe Experience Manager apresenta o suporte do Admin Console para instâncias do AEM e a autenticação baseada no Adobe IMS (Identity Management System) para AEM no Managed Services.   Essa integração permite que os clientes do AEM Managed Services gerenciem todos os usuários do Experience Cloud em um único console unificado da Web. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias do AEM, concedendo acesso gerenciado centralmente às instâncias específicas do AEM.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Noções básicas sobre a autenticação do Adobe IMS com o AEM no Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

O Adobe Experience Manager apresenta o suporte do Admin Console para instâncias do AEM e a autenticação do Adobe Identity Management System (IMS) para o AEM no Managed Services.   Essa integração permite que os clientes do AEM Managed Services gerenciem todos os usuários do Experience Cloud em um único console unificado da Web. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias do AEM, concedendo acesso gerenciado centralmente às instâncias específicas do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/327599?quality=12&learn=on&captions=por_br)

* O suporte à autenticação do Adobe Experience Manager IMS é somente para usuários &quot;internos&quot; (autores, revisores, administradores, desenvolvedores e assim por diante) e não para usuários finais externos, como visitantes de sites.
* O [Admin Console](https://adminconsole.adobe.com/) representa os clientes do AEM Managed Services como Organizações IMS e as instâncias do AEM como Contextos de Produto. Os administradores de sistema e de produto da Admin Console podem definir e gerenciar.
* O AEM Managed Services sincroniza sua topologia com o Admin Console, criando um mapeamento de 1 para 1 entre um Contexto de produto e uma instância do AEM.
* O Perfil de produto no Admin Console determina quais instâncias do AEM um usuário pode acessar.
* O suporte à autenticação inclui IDPs compatíveis com SAML2 do cliente para SSO.
* Somente IDs Enterprise ou Federated (para SSO do cliente) são compatíveis (as IDs Adobe pessoais não são compatíveis).

*&#42;Este recurso é suportado pelo AEM 6.4 SP3 e posterior para clientes do Adobe Managed Services.*

## Práticas recomendadas {#best-practices}

### Aplicação de permissões no Admin Console

A aplicação de permissões e acesso no nível do usuário deve ser evitada no Admin Console e no Adobe Experience Manager.

No Admin Console, os usuários devem receber acesso por meio de Grupos de usuários no nível do Contexto do produto. Normalmente, os grupos de usuários são melhor expressos pela função lógica na organização para promover a reutilização dos grupos em todos os produtos da Adobe Experience Cloud.

### Aplicação de permissões no Adobe Experience Manager

No Adobe Experience Manager, os grupos de usuários sincronizados do Adobe IMS devem estar no termo adicionado aos [grupos de usuários fornecidos pela AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=pt-BR), que vêm pré-configurados com as permissões apropriadas para executar conjuntos específicos de tarefas no AEM. Os usuários sincronizados do Adobe IMS não devem ser adicionados diretamente aos [grupos de usuários fornecidos pela AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=pt-BR).
