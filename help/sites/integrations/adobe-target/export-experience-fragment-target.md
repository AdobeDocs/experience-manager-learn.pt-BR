---
title: Exporte fragmentos de experiência do para o Adobe Target
description: Saiba como publicar e exportar AEM fragmento de experiência como ofertas do Adobe Target.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 5%

---

# Exportar fragmento de experiência para o Adobe Target {#experience-fragment-target}

Saiba como exportar AEM Fragmento de experiência como ofertas do Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Próximas etapas

+ [Criar uma atividade do Target usando ofertas de fragmento de experiência](./create-target-activity.md)

## Resolução de problemas

### Falha na exportação de fragmentos de experiência para o Target

#### Erro

Exportar o Fragmento de experiência para o Adobe Target sem as permissões corretas no Adobe Admin Console resulta no seguinte erro no serviço de Autor do AEM:

    ![Erro da interface do usuário da API do Target](assets/error-target-offer.png)

... e as seguintes mensagens de log no log `aemerror`:

    ![Erro do Console da API do Target](assets/target-console-error.png)

#### Resolução

1. Faça logon em [Admin Console](https://adminconsole.adobe.com/) com direitos administrativos para o Perfil de produto Adobe Target usado, mas a integração de AEM
2. Selecione __Produtos > Adobe Target > Perfil de produto__
3. Na guia __Integrations__, selecione a integração do AEM como um ambiente Cloud Service (o mesmo nome do projeto Adobe I/O)
4. Atribuir função __Editor__ ou __Aprovador__

   ![Erro da API do Target](assets/target-permissions.png)

Adicionar a permissão correta à integração do Adobe Target resolverá esse erro.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
