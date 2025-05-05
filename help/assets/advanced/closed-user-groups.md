---
title: Grupos de usuários fechados no AEM Assets
description: Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com o Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Grupos de usuários fechados{#using-closed-user-groups-with-aem-assets}

Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com o Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos. O suporte para grupos de usuários fechados com o AEM Assets foi introduzido pela primeira vez no AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Grupo de usuários fechado (CUG) com o AEM Assets

* Projetado para restringir o acesso a ativos em uma instância de publicação do AEM.
* Concede acesso de leitura a um conjunto de usuários/grupos.
* O CUG só pode ser configurado no nível da pasta. O CUG não pode ser definido em ativos individuais.
* As políticas CUG são automaticamente herdadas por qualquer subpasta e ativo aplicado.
* As políticas CUG podem ser substituídas por subpastas definindo uma nova política CUG. Isso deve ser usado com moderação e não é considerado uma prática recomendada.

## Grupos de usuários fechados vs. Listas de controle de acesso {#closed-user-groups-vs-access-control-lists}

Tanto o CUG (Grupos de usuários fechados) quanto as ACLs (Listas de controle de acesso) são usados para controlar o acesso ao conteúdo no AEM e com base nos usuários e grupos de segurança do AEM. No entanto, a aplicação e a implementação desses recursos são muito diferentes. A tabela a seguir resume as distinções entre os dois recursos.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso Pretendido | Configure e aplique permissões para o conteúdo na instância do AEM **current**. | Configure as políticas CUG para o conteúdo na instância **author** do AEM. Aplique políticas CUG para conteúdo nas instâncias do AEM **publish**. |
| Níveis de permissão | Define permissões concedidas/negadas para usuários/grupos para todos os níveis: Ler, Modificar, Criar, Excluir, Ler ACL, Editar ACL, Replicar. | Concede acesso de leitura a um conjunto de usuários/grupos. Nega acesso de leitura a *todos os outros* usuários/grupos. |
| Publicação | As ACLs estão *não* publicadas com conteúdo. | As políticas CUG *são* publicadas com conteúdo. |

## Links de suporte {#supporting-links}

* [Gerenciamento de Assets e grupos de usuários fechados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=pt-BR#closed-user-group)
* [Criando um Grupo de Usuários Fechado](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=pt-BR)
* [Documentação de grupo de usuários fechado do Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
