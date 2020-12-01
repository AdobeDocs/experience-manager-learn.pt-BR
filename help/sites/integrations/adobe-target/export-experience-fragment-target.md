---
title: Exporte fragmentos de experiência do para o Adobe Target
description: Saiba como publicar e exportar AEM Fragmento de experiência como Ofertas Adobe Target.
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 4%

---


# Exportar fragmento de experiência para Adobe Target {#experience-fragment-target}

Saiba como exportar AEM Fragmento de experiência como Adobe Target Oferta.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Próximas etapas

+ [Criar uma Atividade de Público alvo usando Ofertas de fragmento de experiência](./create-target-activity.md)

## Resolução de problemas

### Falha ao exportar fragmentos de experiência para o Público alvo

#### Erro

Exportar o fragmento de experiência para o Adobe Target sem as permissões corretas no Adobe Admin Console resulta no seguinte erro no serviço de autor de AEM:

    ![Erro de interface de usuário da API do Público alvo](assets/error-target-offer.png)

... e as seguintes mensagens de registro no registro `aemerror`:

    ![Erro do console da API do Público alvo](assets/target-console-error.png)

#### Resolução

1. Faça logon em [Admin Console](https://adminconsole.adobe.com/) com direitos administrativos para o Perfil de produto Adobe Target usado, mas a integração AEM
2. Selecione __Produtos > Adobe Target > Perfil do produto__
3. Na guia __Integrations__, selecione a integração do seu AEM como ambiente Cloud Service (o mesmo nome do projeto Adobe I/O)
4. Atribuir função __Editor__ ou __Aprovador__

   ![Erro de API de público alvo](assets/target-permissions.png)

A adição da permissão correta à sua integração com o Adobe Target resolverá esse erro.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)