---
title: Exportar fragmentos de experiência para o Adobe Target
description: Saiba como publicar e exportar o fragmento de experiência do AEM como ofertas do Adobe Target.
feature: Experience Fragments
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 3%

---

# Exportar fragmento de experiência para o Adobe Target {#experience-fragment-target}

Saiba como exportar fragmentos de experiência do AEM como ofertas do Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/328975?quality=12&learn=on&captions=por_br)

## Próximas etapas

+ [Criar uma atividade do Target usando ofertas de fragmento de experiência](./create-target-activity.md)

## Resolução de problemas

### Falha ao exportar fragmentos de experiência para o Target

#### Erro

A exportação do fragmento de experiência para o Adobe Target sem as permissões corretas no Adobe Admin Console resulta no seguinte erro no serviço do Autor do AEM:

![Erro na interface da API do Target](assets/error-target-offer.png)

... e as seguintes mensagens de log no log `aemerror`:

![Erro de Console da API de Destino](assets/target-console-error.png)

#### Resolução

1. Logon no [Admin Console](https://adminconsole.adobe.com/) com direitos administrativos para o Perfil de Produto da Adobe Target usado, mas a integração do AEM
2. Selecione __Produtos > Adobe Target > Perfil de produto__
3. Na guia __Integrações__, selecione a integração para seu ambiente do AEM as a Cloud Service (mesmo nome do projeto do Adobe Developer)
4. Atribuir função de __Editor__ ou __Aprovador__

   ![Erro na API de Destino](assets/target-permissions.png)

Adicionar a permissão correta à integração do Adobe Target deve resolver esse erro.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
