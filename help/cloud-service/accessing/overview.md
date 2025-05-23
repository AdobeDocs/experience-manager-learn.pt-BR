---
title: Configuração do acesso ao AEM as a Cloud Service
description: O AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos da AEM e, como tal, usa o Adobe IMS (Identity Management System) para facilitar o logon de usuários, tanto administradores quanto usuários regulares, no serviço de autor do AEM. Saiba como usuários, grupos de usuários e perfis de produtos do Adobe IMS são usados juntamente com grupos e permissões do AEM para fornecer acesso específico ao AEM Author.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 26%

---

# Configuração do acesso ao AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introdução ao Adobe IMS"
>abstract="O AEM as a Cloud Service utiliza o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, tanto administradores quanto usuários comuns, no serviço do AEM Author. Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço AEM Author."

O AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos da AEM e, como tal, usa o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, administradores e usuários regulares, no serviço de autor do AEM.

![Adobe Admin Console](./assets/hero.png)

Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço AEM Author.

## Usuários do Adobe IMS

Os usuários que precisam de acesso ao serviço de Autor do AEM são gerenciados como [usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html) no [Admin Console da Adobe](https://adminconsole.adobe.com). Saiba mais sobre o que são usuários do Adobe IMS e como eles são acessados e gerenciados no Admin Console.

>[!NOTE]
>
>Quando um usuário IMS é excluído do Admin Console, ele não é excluído automaticamente do AEM, mas depois que a sessão (token) do AEM expira, ele NÃO pode fazer logon no AEM.


[Saiba mais sobre os usuários do Adobe IMS](./adobe-ims-users.md)

## Grupos de usuários do Adobe IMS

Os usuários que acessam o serviço AEM Author devem ser organizados em grupos lógicos usando [grupos de usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/user-groups.html) no [Admin Console da Adobe](https://adminconsole.adobe.com). Os grupos de usuários do Adobe IMS não fornecem permissões diretas ou acesso ao AEM (este é o trabalho dos [perfis de produtos do Adobe IMS](#adobe-ims-product-profiles)). No entanto, eles são uma ótima maneira de definir agrupamentos lógicos de usuários que podem ser traduzidos para níveis específicos de acesso no serviço do AEM Author, usando grupos e permissões do AEM.

[Saiba mais sobre os grupos de usuários do Adobe IMS](./adobe-ims-user-groups.md)

## Perfis de produtos do Adobe IMS

Os [perfis de produtos do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/manage-permissions-and-roles.html), gerenciados no [Admin Console do Adobe](https://adminconsole.adobe.com), são a mecânica que fornece aos [usuários do Adobe IMS](#adobe-ims-users) acesso para fazer logon no serviço de Autor do AEM com um nível básico de acesso.

+ O perfil de produto __Usuários do AEM__ concede aos usuários acesso somente leitura à AEM por meio da associação ao grupo Colaboradores da AEM.
+ O perfil de produto __Administradores do AEM__ concede aos usuários acesso administrativo total ao AEM.

[Saiba mais sobre perfis de produtos do Adobe IMS](./adobe-ims-product-profiles.md)

## Grupos e permissões de usuários do AEM

O Adobe Experience Manager se baseia em usuários, grupos de usuários e perfis de produtos do Adobe IMS para fornecer aos usuários acesso personalizável ao AEM. Saiba como criar grupos e permissões do AEM e como eles trabalham em conjunto com abstrações do Adobe IMS para fornecer acesso fácil e personalizável ao AEM.

[Saiba mais sobre usuários, grupos e permissões do AEM](./aem-users-groups-and-permissions.md)

## Passo a passo de acesso e permissões

Uma apresentação resumida configurando usuários do Adobe IMS, grupos de usuários e perfis de produtos no Adobe Admin Console e como aproveitar essas abstrações do Adobe IMS no AEM Author para definir e gerenciar permissões específicas baseadas em grupos.

[Apresentação sobre acesso e permissões do AEM](./walk-through.md)

## Recursos adicionais do Adobe Admin Console

A documentação a seguir aborda detalhes e preocupações específicos do [Adobe Admin Console](https://adminconsole.adobe.com) que podem ajudar a entender melhor o Adobe Admin Console e usá-lo para gerenciar usuários e o acesso nos produtos da Experience Cloud.

+ [Visão geral da identidade da Adobe Admin Console](https://helpx.adobe.com/br/enterprise/using/identity.html)
+ [Funções de administrador do Adobe Admin Console](https://helpx.adobe.com/br/enterprise/using/admin-roles.html)
+ [Funções de desenvolvedor do Adobe Admin Console](https://helpx.adobe.com/br/enterprise/using/manage-developers.html)
