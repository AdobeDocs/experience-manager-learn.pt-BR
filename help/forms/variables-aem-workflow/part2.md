---
title: Variáveis no fluxo de trabalho AEM[Parte2]
seo-title: Variáveis no fluxo de trabalho AEM[Parte2]
description: Uso de variáveis do tipo xml,json,arraylist,documento no fluxo de trabalho aem
seo-description: Uso de variáveis do tipo xml,json,arraylist,documento no fluxo de trabalho aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Variáveis do tipo JSON no fluxo de trabalho AEM

A partir do AEM Forms 6.5, agora podemos criar variáveis do tipo JSON no AEM Workflow. Normalmente, você criará variáveis do tipo JSON se estiver enviando um Forms adaptável baseado no schema JSON para um Fluxo de trabalho AEM ou se quiser armazenar os resultados de uma operação Invocar modelo de dados de formulário. O vídeo a seguir o orienta pelas etapas necessárias para criar e usar uma variável do tipo JSON no fluxo de trabalho AEM

**Se estiver usando o AEM Forms 6.5.0**

Quando estiver criando uma variável do tipo JSON para capturar os dados enviados em seu modelo de fluxo de trabalho, não associe o schema JSON à variável. Isso ocorre porque quando você envia o Formulário adaptativo baseado no schema JSON os dados enviados não são compatíveis com o schema JSON. Os dados de reclamação do schema JSON são incluídos no elemento afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Se estiver usando o AEM Forms 6.5.1 e superior**

Você pode mapear o schema com a variável do tipo JSON no modelo de fluxo de trabalho. Você pode usar o navegador de schemas para mapear os elementos do schema com suas variáveis de string/número no modelo de fluxo de trabalho

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para que os ativos funcionem em seu sistema, siga as seguintes etapas:

* [Baixar e importar ativos para AEM usando o gerenciador de pacotes](assets/jsonandstringvariable.zip)
* [Explore o ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) modelo de fluxo de trabalho para entender as variáveis usadas no fluxo de trabalho
* [Configurar o serviço de e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra o formulário adaptativo](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie o formulário
