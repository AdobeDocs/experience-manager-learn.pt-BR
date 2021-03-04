---
title: Exporte fragmentos de experiência do para o Adobe Target
description: Saiba como publicar e exportar o Fragmento de experiência do AEM como ofertas do Adobe Target.
feature: Fragmentos de experiência
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrações
role: Profissional
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 6%

---


# Exportar fragmento de experiência para o Adobe Target {#experience-fragment-target}

Saiba como exportar o Fragmento de experiência do AEM como ofertas do Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Próximas etapas

+ [Criar uma atividade do Target usando ofertas de fragmento de experiência](./create-target-activity.md)

## Resolução de problemas

### Falha na exportação de fragmentos de experiência para o Target

#### Erro

Exportar o fragmento de experiência para o Adobe Target sem as permissões corretas no Adobe Admin Console resulta no seguinte erro no serviço de autor do AEM:

    ![Erro da interface do usuário da API do Target](assets/error-target-offer.png)

... e as seguintes mensagens de log no log `aemerror`:

    ![Erro do Console da API do Target](assets/target-console-error.png)

#### Resolução

1. Faça logon em [Admin Console](https://adminconsole.adobe.com/) com direitos administrativos para o Perfil de produto do Adobe Target usado, mas a integração do AEM
2. Selecione __Produtos > Adobe Target > Perfil de produto__
3. Na guia __Integrações__ , selecione a integração para o ambiente do AEM as a Cloud Service (o mesmo nome do projeto do Adobe I/O)
4. Atribuir função __Editor__ ou __Aprovador__

   ![Erro da API do Target](assets/target-permissions.png)

Adicionar a permissão correta à integração do Adobe Target deve resolver esse erro.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)