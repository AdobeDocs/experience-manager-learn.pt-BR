---
title: Associar o componente de página ao novo modelo de formulário adaptável
description: Criar um novo componente de página
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 30
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 4%

---

# Associar o componente de página ao modelo

A próxima etapa é associar o componente de Página ao novo modelo de formulário adaptável. Isso garante que o código no componente Página seja executado sempre que um formulário adaptável com base no novo modelo for renderizado. Para o propósito deste tutorial, um novo modelo de formulário adaptável chamado **StoreAndRestoreFromAzure** foi criado na **AzurePortalStorage** pasta.
Navegue até o nó /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content, adicione a seguinte propriedade e salve as alterações.

| **Nome da Propriedade** | **Tipo de propriedade** | **Valor da propriedade** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |

Navegue até o nó /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content, adicione a seguinte propriedade e salve as alterações.
| **Nome da propriedade**  | **Tipo de propriedade** | **Valor da propriedade**                                    | |—|—|—| | sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |


## Próximas etapas

[Criar integração com o Armazenamento do Azure](./create-fdm.md)
