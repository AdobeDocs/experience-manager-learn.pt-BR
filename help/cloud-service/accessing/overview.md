---
title: Configuração do acesso ao AEM como um Cloud Service
description: O AEM como Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos AEM e, como tal, utiliza o Adobe IMS (Identity Management System) para facilitar o logon de usuários, administradores e usuários comuns, no serviço de autor do AEM. Saiba como usuários, grupos de usuários e perfis de produtos do Adobe IMS são usados junto a grupos e permissões de AEM para fornecer acesso específico ao AEM Author.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Configuração do acesso ao AEM como um Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introdução ao Adobe IMS"
>abstract="O AEM as a Cloud Service aproveita o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, tanto administradores quanto usuários comuns, no serviço de Autor do AEM. Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões de AEM para fornecer acesso refinado ao serviço de autor do AEM."

O AEM como Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos AEM e, como tal, utiliza o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, administradores e usuários comuns, no serviço AEM Author.

![Adobe Admin Console](./assets/hero.png)

Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões de AEM para fornecer acesso refinado ao serviço de autor do AEM.

## Usuários de Adobe IMS

Os usuários que exigem acesso ao serviço de autor do AEM são gerenciados como [Adobe IMS users](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html) em [Adobe AdminConsole](https://adminconsole.adobe.com). Saiba mais sobre o que são usuários do Adobe IMS e como eles são acessados e gerenciados no Admin Console.

[Saiba mais sobre usuários do Adobe IMS](./adobe-ims-users.md)

## Grupos de usuários do Adobe IMS

Os usuários que acessam o serviço Autor do AEM devem ser organizados em grupos lógicos usando [Adobe IMS user groups](https://helpx.adobe.com/enterprise/using/user-groups.html) em [Adobe AdminConsole](https://adminconsole.adobe.com). Os grupos de usuários do Adobe IMS não fornecem permissões diretas ou acesso a AEM (essa é a tarefa de [Adobe IMS product profiles](#adobe-ims-product-profiles)), no entanto, são uma ótima maneira de definir agrupamentos lógicos de usuários que podem, por sua vez, ser traduzidos para níveis específicos de acesso no serviço de autor do AEM, usando grupos e permissões AEM.

[Saiba mais sobre grupos de usuários do Adobe IMS](./adobe-ims-user-groups.md)

## Perfis de produto Adobe IMS

[Os perfis de produtos do Adobe IMS ](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gerenciados no  [Admin Console](https://adminconsole.adobe.com), são o mecanismo que fornece aos  [usuários do ](#adobe-ims-users) Adobe IMS acesso para fazer logon no serviço de autor do AEM com um nível básico de acesso.

+ O perfil de produto __AEM Usuários__ oferece aos usuários acesso somente leitura à AEM por meio da associação no grupo AEM Contribuidores .
+ O perfil de produto __AEM Administradores__ oferece aos usuários acesso completo e administrativo ao AEM.

[Saiba mais sobre os perfis de produto do Adobe IMS](./adobe-ims-product-profiles.md)

## AEM usuários, grupos e permissões

O Adobe Experience Manager tem usuários do Adobe IMS, grupos de usuários e perfis de produtos para fornecer aos usuários acesso personalizável ao AEM. Saiba como criar grupos e permissões de AEM e como eles trabalham em conjunto com abstrações do Adobe IMS para fornecer acesso fácil e personalizável à AEM.

[Saiba mais sobre AEM usuário, grupos e permissões](./aem-users-groups-and-permissions.md)

## Acesso e permissões

Uma apresentação resumida configurando usuários do Adobe IMS, grupos de usuários e perfis de produtos no Adobe Admin Console, e como aproveitar essas abstrações do Adobe IMS no AEM Author para definir e gerenciar permissões específicas baseadas em grupos.

[Acesso AEM e permissões](./walk-through.md)

## Recursos adicionais do Adobe Admin Console

A documentação a seguir cobre detalhes e preocupações específicos do [Adobe Admin Console](https://adminconsole.adobe.com) que podem ajudar a entender melhor a Adobe Admin Console e usá-la para gerenciar usuários e acessar produtos do Experience Cloud.

+ [Visão geral da Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Funções administrativas do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funções do desenvolvedor do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
