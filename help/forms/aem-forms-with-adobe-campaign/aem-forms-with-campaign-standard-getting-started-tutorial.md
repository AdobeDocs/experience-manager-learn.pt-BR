---
title: Introdução ao AEM Forms e Adobe Campaign Standard
description: Integre o AEM Forms com o Adobe Campaign Standard usando o modelo de dados do formulário do AEM Forms para buscar informações de perfil da campanha do ACS etc.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Introdução ao AEM Forms e Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial listará os vários casos de uso para integrar o AEM Forms com o Adobe Campaign Standard(ACS).

O ACS tem um conjunto avançado de APIs expostas, o que permite a interface do ACS com a tecnologia de nossa escolha. Neste tutorial, nos concentraremos na interface do AEM Forms com o ACS.

Para integrar o AEM Forms com ACS, você precisará seguir as seguintes etapas:

* [Configure o acesso à API na sua instância do ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* Criar JSON Web Token.
* Troque o JSON Web Token pelo serviço Adobe Identity Management por um token de acesso.
* Inclua esse Token de acesso no Cabeçalho HTTP de autorização, juntamente com a X-API-Key em cada solicitação para a instância ACS.

Para começar, siga as instruções a seguir

* [Baixe e descompacte os ativos relacionados a este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implante os pacotes usando [Felix web console](http://localhost:4502/system/console/bundles)
* Forneça as configurações apropriadas para o Adobe Campaign na configuração Felix OSGI.
* [Crie um usuário de serviço conforme mencionado neste artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Implante o pacote OSGi associado ao artigo.
* Armazene a chave privada do ACS em etc/key/campaign/private.key. Você terá que criar uma pasta chamada campanha em etc/key.
* [Forneça acesso de leitura à pasta da campanha para os &quot;dados&quot; do usuário do serviço.](http://localhost:4502/useradmin)

## Próximas etapas

[Gerar JWT e token de acesso](partone.md)
