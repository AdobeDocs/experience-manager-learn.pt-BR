---
title: Configuração do acesso ao AEM as a Cloud Service
description: O AEM as a Cloud Service é a maneira nativa da nuvem de utilizar os aplicativos do AEM, que usa o Adobe IMS (Identity Management System) para facilitar o logon de usuários, tanto administradores quanto usuários regulares, no serviço do AEM Author. Saiba como usuários, grupos de usuários e perfis de produtos do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso específico ao AEM Author.
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
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# Configuração do acesso ao AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introdução ao Adobe IMS"
>abstract="O AEM as a Cloud Service utiliza o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, tanto administradores quanto usuários comuns, no serviço do AEM Author. Saiba como usuários, grupos e perfis de produtos do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço do AEM Author."

O AEM as a Cloud Service é a maneira nativa na nuvem de utilizar os aplicativos do AEM, que usa o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, sejam administradores ou usuários normais, no serviço do AEM Author.

![Adobe Admin Console](./assets/hero.png)

Saiba como usuários, grupos e perfis de produtos do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço do AEM Author.

## Usuários do Adobe IMS

Os usuários que precisam de acesso ao serviço do AEM Author são gerenciados como [usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html) no [Adobe Admin Console](https://adminconsole.adobe.com). Saiba mais sobre o que são usuários do Adobe IMS e como eles são acessados e gerenciados no Admin Console.

>[!NOTE]
>
>Quando um usuário do IMS é excluído do Admin Console, ele não é excluído automaticamente do AEM, mas, uma vez expirada a sessão (token) do AEM, ele NÃO pode fazer logon no AEM.


[Saiba mais sobre os usuários do Adobe IMS](./adobe-ims-users.md)

## Grupos de usuários do Adobe IMS

Os usuários que acessam o serviço do AEM Author devem ser organizados em grupos lógicos, usando-se [grupos de usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/user-groups.html) no [Adobe Admin Console](https://adminconsole.adobe.com). Os grupos de usuários do Adobe IMS não fornecem permissões diretas nem acesso ao AEM (esse trabalho cabe aos [perfis de produtos do Adobe IMS](#adobe-ims-product-profiles)). No entanto, eles são uma ótima maneira de definir agrupamentos lógicos de usuários, que podem ser traduzidos em níveis específicos de acesso no serviço do AEM Author, usando-se grupos e permissões do AEM.

[Saiba mais sobre os grupos de usuários do Adobe IMS](./adobe-ims-user-groups.md)

## Perfis de produtos do Adobe IMS

Os [perfis de produtos do Adobe IMS](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), gerenciados no [Adobe Admin Console](https://adminconsole.adobe.com), são a mecânica que fornece aos [usuários do Adobe IMS](#adobe-ims-users) acesso para fazer logon no serviço do AEM Author com um nível básico de acesso.

+ O perfil de produto __Usuários do AEM__ concede aos usuários acesso somente de leitura ao AEM por meio da associação ao grupo de colaboradores do AEM.
+ O perfil de produto __Administradores do AEM__ concede aos usuários acesso administrativo total ao AEM.

[Saiba mais sobre os perfis de produtos do Adobe IMS](./adobe-ims-product-profiles.md)

## Grupos de usuários e permissões do AEM

O Adobe Experience Manager se baseia em usuários, grupos de usuários e perfis de produtos do Adobe IMS para fornecer aos usuários acesso personalizável ao AEM. Aprenda a criar grupos e permissões do AEM e saiba como eles trabalham em conjunto com abstrações do Adobe IMS para fornecer um acesso fácil e personalizável ao AEM.

[Saiba mais sobre usuários, grupos e permissões do AEM](./aem-users-groups-and-permissions.md)

## Passo a passo de acesso e permissões

Um guia resumido para configurar usuários, grupos de usuários e perfis de produtos do Adobe IMS no Adobe Admin Console. O guia também mostra como utilizar essas abstrações do Adobe IMS no AEM Author para definir e gerenciar permissões específicas baseadas em grupos.

[Passo a passo de acesso e permissões do AEM](./walk-through.md)

## Recursos adicionais do Adobe Admin Console

A documentação a seguir apresenta informações e questões específicas do [Adobe Admin Console](https://adminconsole.adobe.com). Ajuda a entender melhor e a usar o Adobe Admin Console para gerenciar usuários e acessos em todos os produtos da Experience Cloud.

+ [Visão geral da identidade no Adobe Admin Console](https://helpx.adobe.com/br/enterprise/using/identity.html)
+ [Funções de administrador no Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funções de desenvolvedor no Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)
