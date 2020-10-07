---
title: Grupos de usuários fechados no AEM Assets
description: 'Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com os Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos. O suporte para grupos de usuários fechados com o AEM Assets foi introduzido pela primeira vez no AEM 6.4. '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---


# Grupos de usuários fechados{#using-closed-user-groups-with-aem-assets}

Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com os Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos. O suporte para grupos de usuários fechados com o AEM Assets foi introduzido pela primeira vez no AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## CUG (Closed User Group, grupo de usuários fechado) com AEM Assets

* Projetado para restringir o acesso aos ativos em uma instância do AEM Publish.
* Concede acesso de leitura a um conjunto de usuários/grupos.
* O CUG só pode ser configurado no nível da pasta. CUG não pode ser definido em ativos individuais.
* As políticas de CUG são herdadas automaticamente por qualquer subpasta e ativos aplicados.
* As políticas de CUG podem ser substituídas por subpastas ao configurar uma nova política de CUG. Esta deve ser utilizada com moderação e não é considerada uma prática recomendada.

## Representação de CUG no JCR {#cug-representation-in-the-jcr}

![Representação de CUG no JCR](assets/closed-user-groups/folder-properties-closed-user-groups.png)

Grupo de membros We.Retail adicionado como um Grupo de usuários fechado à pasta: /content/dam/we-retail/en/beta-products

Uma mistura de **rep:CugMixin** é aplicada à pasta **/content/dam/we-retail/en/beta-products** . Um nó de **rep:cugPolicy** é adicionado abaixo da pasta e nós-retail-Members é especificado como principal. Outra mistura de **granite:AuthenticationRequired** é aplicada à pasta de produtos beta e a propriedade* granite*:loginPath** especifica a Página de login a ser usada se um usuário não estiver autenticado e tentar solicitar um ativo abaixo da pasta de produtos **** beta.

Descrição do JCR abaixo:

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## Grupos de usuários fechados vs. Listas de Controles de acesso {#closed-user-groups-vs-access-control-lists}

Os Grupos de usuários fechados (CUG) e as Listas de Controles de acesso (ACL) são usados para controlar o acesso ao conteúdo em AEM e com base em usuários e grupos AEM Segurança. No entanto, a aplicação e implementação desses recursos é muito diferente. O quadro seguinte resume as distinções entre as duas características.

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Utilização prevista | Configure e aplique permissões para conteúdo na instância de AEM **atual** . | Configure as políticas de CUG para conteúdo AEM instância do **autor** . Aplique políticas CUG para conteúdo em AEM instância(s) de **publicação** (s). |
| Níveis de permissão | Define permissões concedidas/negadas para usuários/grupos para todos os níveis: Ler, Modificar, Criar, Excluir, Ler ACL, Editar ACL, Replicar. | Concede acesso de leitura a um conjunto de usuários/grupos. Nega acesso de leitura a todos os outros usuários/grupos. |
| Replicação | As ACLs não são replicadas com conteúdo. | As políticas de CUG são replicadas com conteúdo. |

## Links de suporte {#supporting-links}

* [Gerenciamento de ativos e grupos de usuários fechados](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [Criando um grupo de usuários fechado](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Documentação do Oak Closed User Group](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
