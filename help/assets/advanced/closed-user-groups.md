---
title: Grupos de usuários fechados no AEM Assets
description: Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com os Ativos da Adobe Experience Manager para restringir o acesso a uma pasta específica de ativos.
version: 6.3, 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Grupos de usuários fechados{#using-closed-user-groups-with-aem-assets}

Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com os Ativos da Adobe Experience Manager para restringir o acesso a uma pasta específica de ativos. O suporte para grupos de usuários fechados com o AEM Assets foi introduzido pela primeira vez no AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Grupo de Usuários Fechado (CUG) com AEM Assets

* Projetado para restringir o acesso a ativos em uma instância do AEM Publish.
* Concede acesso de leitura a um conjunto de usuários/grupos.
* CUG só pode ser configurado em um nível de pasta. CUG não pode ser definido em ativos individuais.
* As políticas de CUG são herdadas automaticamente por qualquer subpasta e ativos aplicados.
* As políticas de CUG podem ser substituídas por subpastas definindo uma nova política de CUG. Isso deve ser usado com moderação e não é considerado uma prática recomendada.

## Grupos de Usuários Fechados vs. Listas de Controle de Acesso {#closed-user-groups-vs-access-control-lists}

Os Grupos de Usuários Fechados (CUG) e Listas de Controle de Acesso (ACL) são usados para controlar o acesso ao conteúdo em AEM e com base em usuários e grupos de Segurança AEM. No entanto, a aplicação e a implementação desses recursos são muito diferentes. O quadro seguinte resume as distinções entre as duas características.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilização prevista | Configure e aplique permissões para conteúdo na instância de AEM **atual**. | Configure as políticas de CUG para conteúdo em AEM instância **author**. Aplique políticas CUG para conteúdo em AEM instância **publish**. |
| Níveis de permissão | Define permissões concedidas/negadas para usuários/grupos para todos os níveis: Ler, Modificar, Criar, Excluir, Ler ACL, Editar ACL, Replicar. | Concede acesso de leitura a um conjunto de usuários/grupos. Nega acesso de leitura a *todos os outros* usuários/grupos. |
| Publicação | As ACLs são *não* publicadas com conteúdo. | As políticas de CUG *são* publicadas com conteúdo. |

## Links de suporte {#supporting-links}

* [Gerenciar ativos e grupos de usuários fechados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Criando um grupo de usuários fechado](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentação de grupo de usuários fechado do Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
