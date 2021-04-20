---
title: Configuração do acesso ao AEM as a Cloud Service
description: 'O AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos do AEM e, como tal, usa o Adobe IMS (Identity Management System) para facilitar o logon de usuários, administradores e usuários regulares, no serviço de autor do AEM. Saiba como usuários, grupos de usuários e perfis de produtos do Adobe IMS são usados junto a grupos e permissões do AEM para fornecer acesso específico ao AEM Author.  '
feature: Users and Groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: Administration, Security
role: Administrator
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# Configuração do acesso ao AEM as a Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Introdução ao Adobe IMS"
>abstract="O AEM as a Cloud Service aproveita o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, tanto administradores quanto usuários comuns, no serviço de autor do AEM. Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço de autor do AEM."

O AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos do AEM e, como tal, usa o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, administradores e usuários regulares, no serviço de autor do AEM. Saiba como usuários, grupos e perfis de produto do Adobe IMS são usados em conjunto com grupos e permissões do AEM para fornecer acesso simplificado ao serviço de autor do AEM.

## Usuários do Adobe IMS

Os usuários que exigem acesso ao serviço de autor do AEM são gerenciados como [usuários do Adobe IMS](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html) em [Admin Console](https://adminconsole.adobe.com) da Adobe. Saiba mais sobre o que são usuários do Adobe IMS e como eles são acessados e gerenciados no Admin Console.

[Saiba mais sobre usuários do Adobe IMS](./adobe-ims-users.md)

## Grupos de usuários do Adobe IMS

Os usuários que acessam o serviço Autor do AEM devem ser organizados em grupos lógicos usando [grupos de usuários do Adobe IMS](https://helpx.adobe.com/enterprise/using/user-groups.html) em [Admin Console da Adobe](https://adminconsole.adobe.com). Os grupos de usuários do Adobe IMS não fornecem permissões diretas ou acesso ao AEM (essa é a tarefa de [Adobe IMS product profiles](#adobe-ims-product-profiles)), no entanto, são uma ótima maneira de definir agrupamentos lógicos de usuários que podem, por sua vez, ser traduzidos para níveis específicos de acesso no serviço de autor do AEM, usando grupos e permissões do AEM.

[Saiba mais sobre grupos de usuários do Adobe IMS](./adobe-ims-user-groups.md)

## Perfis de produto do Adobe IMS

[Os perfis](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html) de produtos do Adobe IMS, gerenciados no Admin Console [ da ](https://adminconsole.adobe.com)Adobe, são o mecanismo que fornece aos usuários do  [Adobe IMS ](#adobe-ims-users) acesso para fazer logon no serviço AEM Author com um nível básico de acesso.

+ O perfil de produto __Usuários do AEM__ oferece aos usuários acesso somente leitura ao AEM por meio da associação ao grupo de contribuidores do AEM.
+ O perfil de produto __Administradores do AEM__ oferece aos usuários acesso completo e administrativo ao AEM.

[Saiba mais sobre os perfis de produto do Adobe IMS](./adobe-ims-product-profiles.md)

## Grupos e permissões de usuários do AEM

O Adobe Experience Manager tem usuários do Adobe IMS, grupos de usuários e perfis de produtos para fornecer aos usuários acesso personalizável ao AEM. Saiba como criar grupos e permissões do AEM e como eles trabalham em conjunto com abstrações do Adobe IMS para fornecer acesso fácil e personalizável ao AEM.

[Saiba mais sobre usuários, grupos e permissões do AEM](./aem-users-groups-and-permissions.md)

## Acesso e permissões

Uma apresentação resumida configurando usuários do Adobe IMS, grupos de usuários e perfis de produtos no Adobe Admin Console, e como aproveitar essas abstrações do Adobe IMS no AEM Author para definir e gerenciar permissões específicas baseadas em grupos.

[Permissões e acesso ao AEM](./walk-through.md)

## Recursos adicionais do Adobe Admin Console

A documentação a seguir aborda detalhes e preocupações específicos do [Adobe Admin Console](https://adminconsole.adobe.com) que podem ajudar a entender melhor o Adobe Admin Console e usá-lo para gerenciar usuários e acessar produtos da Experience Cloud.

+ [Visão geral da identidade do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Funções administrativas do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funções do desenvolvedor do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)