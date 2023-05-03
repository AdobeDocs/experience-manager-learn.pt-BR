---
title: Noções básicas sobre a autenticação do Adobe IMS com o AEM no Adobe Managed Services
description: A Adobe Experience Manager apresenta o suporte Admin Console para instâncias de AEM e a autenticação baseada no Adobe IMS (Identity Management System) para AEM no Managed Services.   Essa integração permite que AEM clientes do Managed Services gerenciem todos os usuários do Experience Cloud em um único console da Web unificado. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias de AEM, concedendo acesso gerenciado centralmente às instâncias de AEM específicas.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# Noções básicas sobre a autenticação do Adobe IMS com o AEM no Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

A Adobe Experience Manager apresenta suporte Admin Console para instâncias de AEM e autenticação baseada no Adobe Identity Management System (IMS) para AEM no Managed Services.   Essa integração permite que AEM clientes do Managed Services gerenciem todos os usuários do Experience Cloud em um único console da Web unificado. Os usuários e grupos podem ser atribuídos a perfis de produto associados a instâncias de AEM, concedendo acesso gerenciado centralmente às instâncias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* O suporte de autenticação do Adobe Experience Manager IMS é somente para usuários &quot;internos&quot; (autores, revisores, administradores, desenvolvedores e assim por diante) e não para usuários finais externos, como visitantes do site.
* [Admin Console](https://adminconsole.adobe.com/) representa clientes AEM Managed Services como Orgs do IMS e as instâncias AEM como Contextos do produto. Os Admin Console System e Product Admins podem definir e gerenciar.
* AEM Managed Services sincronize sua topologia com o Admin Console, criando um mapeamento de 1 para 1 entre um Contexto de produto e uma instância AEM.
* O Perfil do produto no Admin Console determina quais instâncias de AEM um usuário pode acessar.
* O suporte de autenticação inclui IDPs compatíveis com SAML2 do cliente para SSO.
* Somente as Enterprise ID ou Federated ID (para SSO do cliente) são compatíveis (as IDs de Adobe pessoal não são compatíveis).

*&#42;Esse recurso é compatível com o AEM 6.4 SP3 e posterior para clientes do Adobe Managed Services.*

## Práticas recomendadas {#best-practices}

### Aplicação de permissões no Admin Console

A aplicação de permissões e acesso no nível do usuário deve ser evitada no Admin Console e no Adobe Experience Manager.

No, os usuários do Admin Console devem receber acesso por meio de Grupos de usuários no nível Contexto do Produto. Em geral, os grupos de usuários são melhor expressos pela função lógica na organização para promover a capacidade de reutilização dos grupos em produtos da Adobe Experience Cloud.

>[!NOTE]
>
> Se estiver usando AEM as a Cloud Service, atribua os usuários do Admin Console diretamente aos Perfis de produto. Não há suporte para permissões transitórias entre usuários do Admin Console para Perfis de produto por meio de grupos de usuários do Admin Console para AEM as a Cloud Service.

### Aplicação de permissões no Adobe Experience Manager

No Adobe Experience Manager, os grupos de usuários sincronizados do Adobe IMS devem ser adicionados por termo ao [Grupos de usuários fornecidos pela AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=pt-BR), que vêm pré-configuradas com as permissões apropriadas para executar conjuntos específicos de tarefas no AEM. Os usuários sincronizados do Adobe IMS não devem ser adicionados diretamente ao [Grupos de usuários fornecidos pela AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=pt-BR).
