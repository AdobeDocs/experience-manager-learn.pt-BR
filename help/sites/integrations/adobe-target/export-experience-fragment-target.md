---
title: Exportar fragmentos de experiência para o Adobe Target
description: Saiba como publicar e exportar o fragmento de experiência do AEM como ofertas do Adobe Target.
feature: Experience Fragments
version: Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 3%

---

# Exportar fragmento de experiência para o Adobe Target {#experience-fragment-target}

Saiba como exportar fragmento de experiência do AEM como ofertas do Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Próximas etapas

+ [Criar uma atividade do Target usando ofertas de fragmento de experiência](./create-target-activity.md)

## Resolução de problemas

### Falha ao exportar fragmentos de experiência para o Target

#### Erro

A exportação do fragmento de experiência para o Adobe Target sem as permissões corretas no Adobe Admin Console resulta no seguinte erro no serviço do autor do AEM:

![Erro na interface da API do Target](assets/error-target-offer.png)

... e as seguintes mensagens de log no log `aemerror`:

![Erro de Console da API de Destino](assets/target-console-error.png)

#### Resolução

1. Logon em [Admin Console](https://adminconsole.adobe.com/) com direitos administrativos para o Perfil de Produto Adobe Target usado, mas a integração do AEM
2. Selecione __Produtos > Adobe Target > Perfil de produto__
3. Na guia __Integrações__, selecione a integração para seu ambiente do AEM as a Cloud Service (mesmo nome do projeto do Adobe Developer)
4. Atribuir função de __Editor__ ou __Aprovador__

   ![Erro na API de Destino](assets/target-permissions.png)

Adicionar a permissão correta à integração do Adobe Target deve resolver esse erro.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
