---
title: Variáveis no fluxo de trabalho do AEM[Part2]
description: Uso de variáveis do tipo XML, JSON, ArrayList, Document em um workflow AEM
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Variáveis do tipo JSON no fluxo de trabalho do AEM

A partir do AEM Forms 6.5, agora podemos criar variáveis do tipo JSON no workflow AEM. Normalmente, você criará variáveis do tipo JSON se estiver enviando o Adaptive Forms com base no esquema JSON para um fluxo de trabalho de AEM ou se quiser armazenar os resultados de uma operação de Chamada do modelo de dados de formulário. O vídeo a seguir mostra as etapas necessárias para criar e usar uma variável do tipo JSON no fluxo de trabalho do AEM

**Se estiver usando o AEM Forms 6.5.0**

Quando estiver criando uma variável do tipo JSON para capturar os dados enviados no modelo de fluxo de trabalho, não associe o esquema JSON à variável. Isso ocorre porque quando você envia o esquema JSON com base no Formulário adaptável, os dados enviados não são compatíveis com o esquema JSON. Os dados de reclamação do esquema JSON são colocados no elemento afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Se estiver usando o AEM Forms 6.5.1 e posterior**

Você pode mapear o esquema com a variável do tipo JSON no modelo de fluxo de trabalho. Em seguida, você pode usar o navegador de esquemas para mapear os elementos do esquema com as variáveis de sequência/número no modelo de fluxo de trabalho

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para fazer com que os ativos funcionem em seu sistema, siga as seguintes etapas:

* [Baixar e importar os ativos para AEM usando o gerenciador de pacotes](assets/jsonandstringvariable.zip)
* [Explore o modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) para entender as variáveis usadas no fluxo de trabalho
* [Configurar o Serviço de email](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abrir o Formulário Adaptável](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie o formulário
