---
title: Configuração do acesso ao AEM as a Cloud Service
description: AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos AEM e, como tal, usa o Adobe IMS (Identity Management System) para facilitar o logon de usuários, administradores e usuários comuns, no serviço de autor do AEM. Saiba como usuários, grupos de usuários e perfis de produtos do Adobe IMS são usados junto a grupos e permissões de AEM para fornecer acesso específico ao AEM Author.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 26%

---

# Configuração do acesso ao AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introdução ao Adobe IMS"
>abstract="O AEM as a Cloud Service utiliza o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, tanto administradores quanto usuários comuns, no serviço do AEM Author. Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço AEM Author."

AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos AEM e, como tal, usa o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, administradores e usuários comuns, no serviço de autor do AEM.

![Adobe Admin Console](./assets/hero.png)

Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço AEM Author.

## Usuários do Adobe IMS

Os usuários que exigem acesso ao serviço de autor do AEM são gerenciados como [Usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html) em [Admin Console](https://adminconsole.adobe.com). Saiba mais sobre o que são usuários do Adobe IMS e como eles são acessados e gerenciados no Admin Console.

>[!NOTE]
>
>Quando um usuário IMS é excluído do Admin Console, ele não é automaticamente excluído do AEM, mas uma vez que AEM sessão (token) expirar, NÃO poderá fazer logon no AEM.


[Saiba mais sobre usuários do Adobe IMS](./adobe-ims-users.md)

## Grupos de usuários do Adobe IMS

Os usuários que acessam o serviço de Autor do AEM devem ser organizados em grupos lógicos usando [Grupos de usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/user-groups.html) em [Admin Console](https://adminconsole.adobe.com). Os grupos de usuários do Adobe IMS não fornecem permissões diretas ou acesso a AEM (essa é a tarefa de [Perfis de produto do Adobe IMS](#adobe-ims-product-profiles)), no entanto, são uma ótima maneira de definir agrupamentos lógicos de usuários que podem, por sua vez, ser traduzidos para níveis específicos de acesso no serviço AEM Author, usando AEM grupos e permissões.

[Saiba mais sobre grupos de usuários do Adobe IMS](./adobe-ims-user-groups.md)

## Perfis de produto do Adobe IMS

[Perfis de produto do Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gerenciado em [Admin Console](https://adminconsole.adobe.com), são o mecanismo que fornece [Usuários do Adobe IMS](#adobe-ims-users) acesso para fazer logon no serviço AEM Author com um nível básico de acesso.

+ O __Usuários AEM__ O perfil de produto oferece aos usuários acesso somente leitura à AEM por meio da associação AEM grupo Contribuidores .
+ O __Administradores de AEM__ O perfil de produto oferece aos usuários acesso total e administrativo ao AEM.

[Saiba mais sobre os perfis de produto do Adobe IMS](./adobe-ims-product-profiles.md)

## AEM usuários, grupos e permissões

O Adobe Experience Manager se baseia em usuários, grupos de usuários e perfis de produtos do Adobe IMS para fornecer aos usuários acesso personalizável ao AEM. Saiba como criar grupos e permissões de AEM e como eles trabalham em conjunto com abstrações do Adobe IMS para fornecer acesso fácil e personalizável à AEM.

[Saiba mais sobre AEM usuário, grupos e permissões](./aem-users-groups-and-permissions.md)

## Acesso e permissões

Uma apresentação resumida configurando usuários do Adobe IMS, grupos de usuários e perfis de produtos no Adobe Admin Console, e como aproveitar essas abstrações do Adobe IMS no AEM Author para definir e gerenciar permissões específicas baseadas em grupos.

[Acesso AEM e permissões](./walk-through.md)

## Recursos adicionais do Adobe Admin Console

A documentação a seguir [Adobe Admin Console](https://adminconsole.adobe.com)Detalhes e preocupações específicos que podem ajudar a entender melhor a Adobe Admin Console e usá-la para gerenciar usuários e acessar produtos de Experience Cloud.

+ [Visão geral da identidade do Adobe Admin Console](https://helpx.adobe.com/br/enterprise/using/identity.html)
+ [Funções administrativas do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funções do desenvolvedor do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
