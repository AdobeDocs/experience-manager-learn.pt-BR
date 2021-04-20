---
title: Introdução ao AEM Forms e Adobe Campaign Standard
seo-title: Introdução ao AEM Forms e Adobe Campaign Standard
description: Integre o AEM Forms ao Adobe Campaign Standard usando o AEM Forms Form Data Model para buscar informações de perfil de campanha ACS etc.
seo-description: Integre o AEM Forms ao Adobe Campaign Standard usando o AEM Forms Form Data Model para buscar informações de perfil de campanha ACS etc.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 0%

---


# Introdução ao AEM Forms e Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial listará os vários casos de uso para integrar o AEM Forms ao Adobe Campaign Standard (ACS).

O ACS tem um conjunto avançado de APIs expostas, que permite que o ACS seja interface com a tecnologia de nossa escolha. Neste tutorial, nos concentraremos em interligar o AEM Forms com ACS.

Para integrar o AEM Forms ao ACS, você precisará seguir as seguintes etapas:

* [Configure o acesso à API na instância do ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Criar JSON Web Token.
* Troque o token da Web JSON pelo Adobe Identity Management Service por um token de acesso.
* Inclua esse Token de acesso no Cabeçalho HTTP de autorização, juntamente com X-API-Key em cada solicitação para a instância ACS.

Para começar, siga as instruções a seguir

* [Baixe e descompacte os ativos relacionados a este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implante os pacotes usando [Felix web console](http://localhost:4502/system/console/bundles)
* Forneça as configurações apropriadas para o Adobe Campaign na Configuração Felix OSGI.
* [Crie um usuário de serviço, conforme mencionado neste artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Certifique-se de implantar o pacote OSGi associado ao artigo.
* Armazene a chave privada ACS em etc/key/campaign/private.key. Será necessário criar uma pasta chamada campanha em etc/key.
* [Forneça acesso de leitura à pasta da campanha para os &quot;dados&quot; do usuário do serviço.](http://localhost:4502/useradmin)
