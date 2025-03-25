---
title: Integrar o AEM Forms e o Adobe Campaign Standard
description: Integre o AEM Forms com o Adobe Campaign Standard usando o modelo de dados do formulário do AEM Forms para buscar informações de perfil da campanha do ACS etc.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 1%

---

# Integrar o AEM Forms e o Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

Saiba como integrar o AEM Forms ao Adobe Campaign Standard (ACS).

O ACS tem um conjunto avançado de APIs expostas, o que permite a interface do ACS com a tecnologia de nossa escolha. Neste tutorial, nos concentraremos na interface do AEM Forms com o ACS.

Para integrar o AEM Forms com ACS, você precisará seguir as seguintes etapas:

* [Configure o acesso à API na sua instância do ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* Criar JSON Web Token.
* Troque o JSON Web Token pelo Adobe Identity Management Service por um token de acesso.
* Inclua esse Token de acesso no Cabeçalho HTTP de autorização, juntamente com a X-API-Key em cada solicitação para a instância ACS.

Para começar, siga as instruções a seguir

* [Baixe e descompacte os ativos relacionados a este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implante os pacotes usando o [Felix web console](http://localhost:4502/system/console/bundles)
* Forneça as configurações apropriadas para o Adobe Campaign na configuração Felix OSGI.
* [Crie um usuário de serviço conforme mencionado neste artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Implante o pacote OSGi associado ao artigo.
* Armazene a chave privada do ACS em etc/key/campaign/private.key. Você terá que criar uma pasta chamada campanha em etc/key.
* [Forneça acesso de leitura à pasta da campanha para os &quot;dados&quot; do usuário do serviço.](http://localhost:4502/useradmin)

## Próximas etapas

[Gerar JWT e token de acesso](partone.md)
