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
duration: 231
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '186'
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

... e as seguintes mensagens de log no `aemerror` registro:

![Erro de console da API de destino](assets/target-console-error.png)

#### Resolução

1. Fazer logon em [Admin Console](https://adminconsole.adobe.com/) com direitos administrativos para o Perfil de produto Adobe Target usado, mas a integração AEM
2. Selecionar __Produtos > Adobe Target > Perfil do produto__
3. Em __Integrações__ , selecione a integração do seu ambiente as a Cloud Service AEM (mesmo nome do projeto do Adobe Developer)
4. Atribuir __Editor__ ou __Aprovador__ função

   ![Erro na API do Target](assets/target-permissions.png)

Adicionar a permissão correta à integração do Adobe Target deve resolver esse erro.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
