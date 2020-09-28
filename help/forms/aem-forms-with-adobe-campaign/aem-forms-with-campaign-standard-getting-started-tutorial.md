---
title: Introdução ao AEM Forms e Adobe Campaign Standard
seo-title: Introdução ao AEM Forms e Adobe Campaign Standard
description: Integre a AEM Forms à Adobe Campaign Standard usando o AEM Forms Form Data Model para obter informações sobre o perfil ACS etc.
seo-description: Integre a AEM Forms à Adobe Campaign Standard usando o AEM Forms Form Data Model para obter informações sobre o perfil ACS etc.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Introdução ao AEM Forms e Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial lista os vários casos de uso para a integração do AEM Forms com o Adobe Campaign Standard(ACS).

O ACS tem um conjunto avançado de APIs expostas, o que permite que o ACS seja integrado à tecnologia de nossa escolha. Neste tutorial, nos concentraremos na interface do AEM Forms com o ACS.

Para integrar o AEM Forms ao ACS, é necessário seguir as seguintes etapas:

* [Configure o acesso à API na instância do ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Criar token da Web JSON.
* Troque o token da Web JSON pelo serviço Adobe Identity Management por um Token de acesso.
* Inclua esse Token de acesso no cabeçalho HTTP de autorização, juntamente com a chave-API-X em cada solicitação para a instância ACS.

Para começar, siga as instruções a seguir

* [Baixe e descompacte os ativos relacionados a este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implantar os pacotes usando o console da Web [Felix](http://localhost:4502/system/console/bundles)
* Forneça as configurações apropriadas para o Adobe Campaign na Configuração do Felix OSGI.
* [Crie um usuário de serviço como mencionado neste artigo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Certifique-se de implantar o pacote OSGi associado ao artigo.
* Armazene a chave privada ACS em etc/key/campaign/private.key. Será necessário criar uma pasta chamada campanha em etc/key.
* [Forneça acesso de leitura à pasta de campanha aos &quot;dados&quot; do usuário do serviço.](http://localhost:4502/useradmin)
