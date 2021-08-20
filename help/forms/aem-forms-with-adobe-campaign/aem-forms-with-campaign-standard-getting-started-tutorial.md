---
title: Introdução ao AEM Forms e Adobe Campaign Standard
description: Integre o AEM Forms ao Adobe Campaign Standard usando o AEM Forms Form Data Model para buscar informações de perfil de campanha ACS etc.
feature: Forms adaptável, Modelo de dados de formulário
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# Introdução ao AEM Forms e Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial listará os vários casos de uso para integração do AEM Forms com o Adobe Campaign Standard (ACS).

O ACS tem um conjunto avançado de APIs expostas, que permite que o ACS seja interface com a tecnologia de nossa escolha. Neste tutorial, nos concentraremos em interligar o AEM Forms com o ACS.

Para integrar o AEM Forms com o ACS, é necessário seguir as seguintes etapas:

* [Configure o acesso à API na instância do ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Criar JSON Web Token.
* Troque o JSON Web Token pelo Adobe Identity Management Service por um Token de acesso.
* Inclua esse Token de acesso no Cabeçalho HTTP de autorização, juntamente com X-API-Key em cada solicitação para a instância ACS.

Para começar, siga as instruções a seguir

* [Baixe e descompacte os ativos relacionados a este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implante os pacotes usando [Felix web console](http://localhost:4502/system/console/bundles)
* Forneça as configurações apropriadas para o Adobe Campaign na Configuração OSGI do Felix.
* [Crie um usuário de serviço, conforme mencionado neste artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Certifique-se de implantar o pacote OSGi associado ao artigo.
* Armazene a chave privada ACS em etc/key/campaign/private.key. Será necessário criar uma pasta chamada campanha em etc/key.
* [Forneça acesso de leitura à pasta da campanha para os &quot;dados&quot; do usuário do serviço.](http://localhost:4502/useradmin)
