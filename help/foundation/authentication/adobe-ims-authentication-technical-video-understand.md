---
title: Compreensão da autenticação Adobe IMS com AEM nos serviços gerenciados da Adobe
description: A Adobe Experience Manager apresenta suporte a Admin Console para instâncias AEM e autenticação baseada no Adobe IMS (Identity Management System) para AEM no Managed Services.   Essa integração permite que AEM clientes Managed Services gerenciem todos os usuários Experience Cloud em um único console da Web unificado. Usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias AEM, concedendo acesso gerenciado centralmente às instâncias AEM específicas.
version: 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---


# Compreensão da autenticação Adobe IMS com AEM nos serviços gerenciados da Adobe{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

A Adobe Experience Manager apresenta suporte a Admin Console para instâncias AEM e autenticação baseada no Adobe IMS (Identity Management System) para AEM no Managed Services.   Essa integração permite que AEM clientes Managed Services gerenciem todos os usuários Experience Cloud em um único console da Web unificado. Os usuários e grupos podem ser atribuídos a perfis de produtos associados a instâncias AEM, concedendo acesso gerenciado centralmente às instâncias AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* O suporte à autenticação do Adobe Experience Manager IMS é somente para usuários &quot;internos&quot; (autores, revisores, administradores, desenvolvedores etc.) e não para usuários finais externos, como visitantes de sites.
* [O Admin ](https://adminconsole.adobe.com/) Consolewill representará AEM clientes Managed Services como Organizações IMS e as instâncias AEM como Contextos de Produto. Os administradores de sistemas e produtos do Admin Console podem definir e gerenciar.
* AEM Managed Services sincroniza sua topologia com Admin Console, criando um mapeamento de 1 para 1 entre um Contexto de produto e uma instância AEM.
* O Perfil de produto no Admin Console determinará quais instâncias AEM um usuário pode acessar.
* O suporte de autenticação inclui IDPs compatíveis com SAML2 do cliente para SSO.
* Somente Enterprise ID ou Federated ID (para SSO do cliente) serão suportadas (as IDs de Adobe pessoal não são suportadas).

** Este recurso é compatível com AEM 6.4 SP3 e posterior para clientes do Adobe Managed Services.*

## Práticas recomendadas {#best-practices}

### Aplicação de permissões no Admin Console

A aplicação de permissões e acesso no nível do usuário deve ser evitada no Admin Console e no Adobe Experience Manager.

No Admin Console, os usuários devem receber acesso por meio de Grupos de usuários no nível Contexto do produto. Em geral, os grupos de usuários são melhor expressos pela função lógica dentro da organização para promover a reutilização dos grupos nos produtos da Adobe Experience Cloud.

>[!NOTE]
>
> Se estiver usando o AEM como um Cloud Service, atribua os usuários do Admin Console diretamente aos Perfis de produtos. As permissões transitórias entre usuários do Admin Console para Perfis de produtos por meio de grupos de usuários do Admin Console não são suportadas para AEM como Cloud Service.

### Aplicar permissões no Adobe Experience Manager

No Adobe Experience Manager, os grupos de usuários sincronizados do Adobe IMS devem ser adicionados por termo a [grupos de usuários fornecidos por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vêm pré-configurados com as permissões apropriadas para executar conjuntos específicos de tarefas no AEM. Os usuários sincronizados de Adobe IMS não devem ser adicionados diretamente a [grupos de usuários fornecidos por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
