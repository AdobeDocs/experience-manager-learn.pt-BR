---
title: Variáveis no fluxo de trabalho AEM[Parte1]
seo-title: Variáveis no fluxo de trabalho AEM[Parte1]
description: Uso de variáveis do tipo xml,json,arraylist,documento no fluxo de trabalho aem
seo-description: Uso de variáveis do tipo xml,json,arraylist,documento no fluxo de trabalho aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Variáveis XML no fluxo de trabalho AEM

Normalmente, as variáveis do tipo XML são usadas quando você tem um Formulário adaptável baseado em XSD e deseja extrair valores do envio do Formulário adaptável no seu fluxo de trabalho.

O vídeo a seguir o orienta pelas etapas necessárias para criar variáveis do tipo String e XML e usá-las no fluxo de trabalho.

A variável XML pode ser usada para preencher previamente o formulário adaptável ou armazenar os dados de envio do formulário adaptável no seu fluxo de trabalho.

A variável String pode ser preenchida por Xpathing na variável XML. Essa variável de string é normalmente usada para preencher os espaços reservados para modelo de email no componente Enviar email

>[!NOTE]
Se o formulário adaptativo não estiver associado ao XSD, o XPath para obter o valor de um elemento será semelhante**a/afData/afUnboundData/data/submitterName**

Os dados do formulário adaptável são armazenados sob o elemento de dados, como mostrado acima. **_No XPath submitterName acima está o nome do campo de texto no Formulário adaptável._**

>[!NOTE]
**AEM Forms 6.5.0** - ao criar uma variável do tipo XML para capturar os dados enviados no modelo de fluxo de trabalho, não associe o XSD à variável. Isso ocorre porque quando você envia o Formulário adaptativo baseado em XSD os dados enviados não são compatíveis com o XSD. Os dados de reclamação XSD são incluídos no elemento /afData/afBoundData/.

**AEM Forms 6.5.1** - se você associar o XSD à sua variável XML, poderá navegar pelos elementos do schema para fazer o mapeamento da variável. Não será possível acessar dados de formulário não vinculados a elementos de schema. Se seu caso de uso for acessar dados vinculados aos elementos do schema, bem como dados não vinculados, não vincule o schema à sua variável XML no fluxo de trabalho.Você precisará usar a expressão XPath apropriada para obter os dados de que precisa

## Criação de variáveis XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Uso do Schema com a variável XML

**Mapeamento de uma variável XML com schema. Use este recurso com o AEM Forms 6.5.1 e versões posteriores***
>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Usar a variável no email de envio

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para que os ativos funcionem em seu sistema, siga as seguintes etapas:

* [Baixar e importar ativos para AEM usando o gerenciador de pacotes](assets/xmlandstringvariable.zip)
* [Explore o modelo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) de fluxo de trabalho para entender as variáveis usadas no fluxo de trabalho
* [Configurar o serviço de e-mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra o formulário adaptativo](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie o formulário.

