---
title: Grupos de usuários fechados no AEM Assets
description: Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com os Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos.
version: 6.3, 6.4, 6.5, cloud-service
topic: Administração, Segurança
feature: Usuário e grupos
role: Admin
level: Intermediário
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---


# Grupos de usuários fechados{#using-closed-user-groups-with-aem-assets}

Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com os Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos. O suporte para grupos de usuários fechados com o AEM Assets foi introduzido pela primeira vez no AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## CUG (Closed User Group, grupo de usuários fechado) com AEM Assets

* Projetado para restringir o acesso aos ativos em uma instância do AEM Publish.
* Concede acesso de leitura a um conjunto de usuários/grupos.
* O CUG só pode ser configurado no nível da pasta. CUG não pode ser definido em ativos individuais.
* As políticas de CUG são herdadas automaticamente por qualquer subpasta e ativos aplicados.
* As políticas de CUG podem ser substituídas por subpastas ao configurar uma nova política de CUG. Esta deve ser utilizada com moderação e não é considerada uma prática recomendada.

## Grupos de usuários fechados vs. Listas de Controles de acesso {#closed-user-groups-vs-access-control-lists}

Os Grupos de usuários fechados (CUG) e as Listas de Controles de acesso (ACL) são usados para controlar o acesso ao conteúdo em AEM e com base em usuários e grupos AEM Segurança. No entanto, a aplicação e implementação desses recursos é muito diferente. O quadro seguinte resume as distinções entre as duas características.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilização prevista | Configure e aplique permissões para conteúdo na instância AEM **atual**. | Configure as políticas de CUG para conteúdo na instância **author** AEM. Aplique políticas CUG para conteúdo em AEM **publicar** instância(s). |
| Níveis de permissão | Define permissões concedidas/negadas para usuários/grupos para todos os níveis: Ler, Modificar, Criar, Excluir, Ler ACL, Editar ACL, Replicar. | Concede acesso de leitura a um conjunto de usuários/grupos. Nega acesso de leitura a *todos os outros* utilizadores/grupos. |
| Publicação | As ACLs são *não* publicadas com conteúdo. | As políticas de CUG *são* publicadas com conteúdo. |

## Links de suporte {#supporting-links}

* [Gerenciamento de ativos e grupos de usuários fechados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Criando um grupo de usuários fechado](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentação do Oak Closed User Group](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
