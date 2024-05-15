---
title: Variáveis no fluxo de trabalho do AEM[Part1]
description: Uso de variáveis do tipo XML, JSON, ArrayList, Document em um workflow AEM
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 561
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Variáveis XML no fluxo de trabalho do AEM

As variáveis do tipo XML normalmente são usadas quando você tem um Formulário adaptável baseado em XSD e deseja extrair valores do envio do Formulário adaptável em seu fluxo de trabalho.

O vídeo a seguir mostra as etapas necessárias para criar variáveis do tipo String e XML e usá-las no fluxo de trabalho.

A variável XML pode ser usada para preencher previamente o formulário adaptável ou armazenar os dados de envio do formulário adaptável no fluxo de trabalho.

A variável de string pode ser preenchida pelo Xpathing na variável XML. Essa variável de sequência é normalmente usada para preencher os espaços reservados para o modelo de email no componente de Envio de email

>[!NOTE]
>
>Se o formulário adaptável não estiver associado ao XSD, o XPath para obter o valor de um elemento será semelhante a
>
>**/afData/afUnboundData/data/submitterName**

Os dados do formulário adaptável são armazenados no elemento de dados, como mostrado acima. **_No XPath submitterName acima, está o nome do campo de texto no Formulário adaptável._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - Quando estiver criando uma variável do tipo XML para capturar os dados enviados no modelo de fluxo de trabalho, não associe o XSD à variável. Isso ocorre porque quando você envia o Formulário adaptável baseado em XSD, os dados enviados não são compatíveis com o XSD. Os dados de reclamação de XSD estão entre o elemento /afData/afBoundData/.
>
>**AEM Forms 6.5.1** - Se associar XSD à variável XML, você poderá navegar pelos elementos do esquema para fazer o mapeamento da variável. Você não poderá acessar dados de formulário não vinculados a elementos do esquema. Se o caso de uso for acessar dados vinculados a elementos de esquema e dados não vinculados, não vincule o esquema à variável XML no fluxo de trabalho. Será necessário usar a expressão XPath apropriada para obter os dados necessários

## Criando variáveis XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Usando esquema com variável XML

**Mapeando uma variável XML com esquema. Usar esse recurso a partir do AEM Forms 6.5.1**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### Uso da variável no envio de email

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para fazer com que os ativos funcionem em seu sistema, siga as seguintes etapas:

* [Baixar e importar os ativos para AEM usando o gerenciador de pacotes](assets/xmlandstringvariable.zip)
* [Explorar o modelo de fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) para entender as variáveis usadas no workflow
* [Configurar o serviço de email](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie o formulário.
