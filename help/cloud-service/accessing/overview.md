---
title: Configurar acesso a AEM como Cloud Service
description: 'AEM como Cloud Service é a forma nativa de nuvem de aproveitar os aplicativos AEM e, como tal, aproveita o Adobe IMS (Identity Management System) para facilitar o logon de usuários, administradores e usuários comuns, no serviço de autor de AEM. Saiba como os usuários, grupos de usuários e perfis de produtos do Adobe IMS são usados em conjunto com grupos e permissões AEM para fornecer acesso específico ao autor do AEM.  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# Configurar acesso a AEM como Cloud Service

AEM como Cloud Service é a forma nativa de nuvem de aproveitar os aplicativos AEM e, como tal, aproveita o Adobe IMS (Identity Management System) para facilitar o logon de seus usuários, administradores e usuários comuns, no serviço de autor de AEM. Saiba como os usuários, grupos e perfis de produtos do Adobe IMS são usados em conjunto com grupos e permissões AEM para fornecer acesso refinado ao serviço de autor de AEM.

## Usuários de Adobe IMS

Os usuários que precisam de acesso ao serviço de autor de AEM são gerenciados como usuários [de](https://helpx.adobe.com/br/enterprise/using/set-up-identity.html) Adobe IMS no [Adobe AdminConsole](https://adminconsole.adobe.com). Saiba mais sobre o que são os usuários de Adobe IMS e como eles são acessados e gerenciados no Admin Console.

[Saiba mais sobre os usuários do Adobe IMS](./adobe-ims-users.md)

## Grupos de usuários de Adobe IMS

Os usuários que acessam o serviço de autor de AEM devem ser organizados em grupos lógicos usando grupos [de usuários](https://helpx.adobe.com/enterprise/using/user-groups.html) Adobe IMS no [AdminConsole](https://adminconsole.adobe.com). Os grupos de usuários do Adobe IMS não fornecem permissões diretas nem acesso a AEM (esse é o trabalho dos perfis [de produtos do](#adobe-ims-product-profiles)Adobe IMS), no entanto, eles são uma excelente forma de definir agrupamentos lógicos de usuários que podem, por sua vez, ser traduzidos para níveis específicos de acesso no serviço de autor de AEM, usando grupos e permissões AEM.

[Saiba mais sobre grupos de usuários do Adobe IMS](./adobe-ims-user-groups.md)

## Perfis de produtos Adobe IMS

[Perfis](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)de produtos Adobe IMS, gerenciados no [Adobe AdminConsole](https://adminconsole.adobe.com), são o mecanismo que fornece aos usuários [do](#adobe-ims-users) Adobe IMS acesso ao serviço de autor de AEM com um nível básico de acesso.

+ O perfil de produtos Usuários ____ AEM oferece aos usuários acesso somente leitura a AEM por meio da associação AEM grupo Contribuidores.
+ O perfil de produtos Administradores __de__ AEM oferece aos usuários acesso administrativo total à AEM.

[Saiba mais sobre os perfis de produtos Adobe IMS](./adobe-ims-product-profiles.md)

## Grupos e permissões de usuários AEM

A Adobe Experience Manager baseia-se em usuários do Adobe IMS, grupos de usuários e perfis de produtos para fornecer aos usuários acesso personalizável ao AEM. Saiba como criar grupos e permissões de AEM e como eles funcionam em conjunto com abstrações de Adobe IMS para fornecer acesso fácil e personalizável a AEM.

[Saiba mais sobre AEM usuário, grupos e permissões](./aem-users-groups-and-permissions.md)

## Passagem de acesso e permissões

Uma apresentação simplificada configurando usuários de Adobe IMS, grupos de usuários e perfis de produtos no Admin Console do Adobe, e como aproveitar essas abstrações de Adobe IMS no Autor do AEM para definir e gerenciar permissões específicas baseadas em grupos.

[Permissões e acesso AEM](./walk-through.md)

## Recursos adicionais da Adobe Admin Console

A documentação a seguir aborda detalhes e preocupações específicos da [Adobe Admin Console](https://adminconsole.adobe.com)que podem ajudar a entender melhor o Adobe Admin Console e usá-lo para gerenciar usuários e acessar produtos do Experience Cloud.

+ [Visão geral do Adobe Admin Console Identity](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Funções de administrador do Adobe Admin Console](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Funções do desenvolvedor Adobe Admin Console](https://helpx.adobe.com/enterprise/using/manage-developers.html)