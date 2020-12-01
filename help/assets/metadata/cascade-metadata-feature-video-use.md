---
title: Uso de metadados em cascata no AEM Assets
seo-title: Uso de metadados em cascata no AEM Assets
description: O gerenciamento avançado de metadados permite que os usuários criem regras de campo em cascata para formar relações contextuais entre os metadados no AEM Assets. O vídeo abaixo demonstra novas regras dinâmicas para requisitos de campo, visibilidade e opções contextuais. O vídeo também detalha as etapas necessárias para que um administrador aplique essas regras a um schema de metadados personalizado.
seo-description: O gerenciamento avançado de metadados permite que os usuários criem regras de campo em cascata para formar relações contextuais entre os metadados no AEM Assets. O vídeo abaixo demonstra novas regras dinâmicas para requisitos de campo, visibilidade e opções contextuais. O vídeo também detalha as etapas necessárias para que um administrador aplique essas regras a um schema de metadados personalizado.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# Uso de metadados em cascata no AEM Assets{#using-cascading-metadata-in-aem-assets}

O gerenciamento avançado de metadados permite que os usuários criem regras de campo em cascata para formar relações contextuais entre os metadados no AEM Assets. O vídeo abaixo demonstra novas regras dinâmicas para requisitos de campo, visibilidade e opções contextuais. O vídeo também detalha as etapas necessárias para que um administrador aplique essas regras a um schema de metadados personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

Há três conjuntos de regras dinâmicas que podem ser ativados para um determinado campo de metadados:

1. **Requisito** : um campo pode ser marcado dinamicamente conforme necessário para se basear no valor de outro campo suspenso.

2. **Visibilidade** : os campos sempre podem ser visíveis ou apenas visíveis com base no valor de outro campo suspenso.

3. **Opções** : (aplicável somente aos campos suspensos) filtram as opções exibidas ao usuário com base no valor atualmente selecionado de outro campo suspenso.

>[!NOTE]
>
>As regras em cascata SÓ podem ser criadas com base nos valores de um campo suspenso. É possível aplicar todos os três conjuntos de regras ao mesmo campo de metadados, mas como prática recomendada, é recomendado tornar cada conjunto de regras dependente da mesma lista suspensa de metadados.

Baixar [Pacote de metadados personalizados](assets/cascade-metadata-values-001.zip)

## Recursos adicionais{#additional-resources}

Schema de metadados personalizados criado em: `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. O pacote AEM abaixo aplicará o schema personalizado à pasta: `/content/dam/we-retail/en/activities`:

