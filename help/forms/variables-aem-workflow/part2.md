---
title: Variáveis no fluxo de trabalho AEM[Parte2]
description: Uso de variáveis do tipo XML, JSON, ArrayList, Documento em um fluxo de trabalho AEM
version: 6.5
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Variáveis do tipo JSON AEM fluxo de trabalho

A partir do AEM Forms 6.5, agora podemos criar variáveis do tipo JSON AEM fluxo de trabalho. Normalmente, você criará variáveis do tipo JSON se estiver enviando um Adaptive Forms com base no esquema JSON para um Fluxo de trabalho AEM ou quiser armazenar os resultados de uma operação Invocar Modelo de dados de formulário. O vídeo a seguir mostra as etapas necessárias para criar e usar uma variável do tipo JSON em AEM fluxo de trabalho

**Se estiver usando o AEM Forms 6.5.0**

Ao criar uma variável do tipo JSON para capturar os dados enviados no modelo de fluxo de trabalho, não associe o esquema JSON à variável . Isso ocorre porque, ao enviar o Formulário adaptável baseado no esquema JSON, os dados enviados não são compatíveis com o esquema JSON. Os dados de reclamação do esquema JSON são colocados no elemento afData.afBoundData.data .

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Se estiver usando o AEM Forms 6.5.1 e superior**

Você pode mapear o esquema com a variável do tipo JSON no modelo de fluxo de trabalho. Em seguida, você pode usar o navegador de esquema para mapear os elementos do esquema com suas variáveis de string/número no modelo de fluxo de trabalho

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Para que os ativos funcionem em seu sistema, siga as seguintes etapas:

* [Baixe e importe os ativos no AEM usando o gerenciador de pacotes](assets/jsonandstringvariable.zip)
* [Explore o ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) modelo de fluxo de trabalho para entender as variáveis usadas no fluxo de trabalho
* [Configurar o serviço de email](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie o formulário
