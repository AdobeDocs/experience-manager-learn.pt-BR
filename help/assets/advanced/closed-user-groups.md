---
title: Grupos de usuários fechados no AEM Assets
description: Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com o Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos.
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 352
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Grupos de usuários fechados{#using-closed-user-groups-with-aem-assets}

Grupos de usuários fechados (CUGs) é um recurso usado para restringir o acesso ao conteúdo a um grupo selecionado de usuários em um site publicado. Este vídeo mostra como Grupos de usuários fechados podem ser usados com o Adobe Experience Manager Assets para restringir o acesso a uma pasta específica de ativos. O suporte para grupos de usuários fechados com AEM Assets foi introduzido pela primeira vez no AEM 6.4.

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Grupo de usuários fechado (CUG) com o AEM Assets

* Projetado para restringir o acesso a ativos em uma instância de publicação do AEM.
* Concede acesso de leitura a um conjunto de usuários/grupos.
* O CUG só pode ser configurado no nível da pasta. O CUG não pode ser definido em ativos individuais.
* As políticas CUG são automaticamente herdadas por qualquer subpasta e ativo aplicado.
* As políticas CUG podem ser substituídas por subpastas definindo uma nova política CUG. Isso deve ser usado com moderação e não é considerado uma prática recomendada.

## Grupos de usuários fechados vs. Listas de controle de acesso {#closed-user-groups-vs-access-control-lists}

Tanto o grupo de usuários fechado (CUG) quanto as listas de controle de acesso (ACL) são usados para controlar o acesso ao conteúdo no AEM e com base nos usuários e grupos de segurança do AEM. No entanto, a aplicação e a implementação desses recursos são muito diferentes. A tabela a seguir resume as distinções entre os dois recursos.

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Uso Pretendido | Configure e aplique permissões para o conteúdo no **atual** Instância do AEM. | Configurar políticas CUG para conteúdo no AEM **autor** instância. Aplicação de políticas CUG para conteúdo no AEM **publicar** instância(s). |
| Níveis de permissão | Define permissões concedidas/negadas para usuários/grupos para todos os níveis: Ler, Modificar, Criar, Excluir, Ler ACL, Editar ACL, Replicar. | Concede acesso de leitura a um conjunto de usuários/grupos. Nega acesso de leitura a *todos os outros* usuários/grupos. |
| Publicação | ACLs são *não* publicado com conteúdo. | Políticas de CUG *são* publicado com conteúdo. |

## Links de suporte {#supporting-links}

* [Gerenciar ativos e grupos de usuários fechados](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [Criação de um grupo fechado de usuários](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Documentação de grupo de usuários fechado do Oak](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
