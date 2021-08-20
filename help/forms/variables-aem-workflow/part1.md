---
title: Variáveis no fluxo de trabalho AEM[Parte1]
description: Uso de variáveis do tipo XML, JSON, ArrayList, Documento em um fluxo de trabalho AEM
feature: Formulários adaptáveis
version: 6.5
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---


# Variáveis XML no fluxo de trabalho AEM

As variáveis do tipo XML geralmente são usadas quando você tem um formulário adaptável baseado em XSD e deseja extrair valores do envio do formulário adaptável no fluxo de trabalho.

O vídeo a seguir o orienta pelas etapas necessárias para criar variáveis do tipo String e XML e usá-las em seu fluxo de trabalho.

A variável XML pode ser usada para preencher previamente o formulário adaptável ou armazenar os dados de envio do formulário adaptável no seu fluxo de trabalho.

A variável de string pode ser preenchida por Xpathing na variável XML. Normalmente, essa variável de string é usada para preencher os espaços reservados do modelo de email no componente Enviar email

>[!NOTE]
>
>Se o formulário adaptável não estiver associado ao XSD, o XPath para obter o valor de um elemento será semelhante
>
>**/afData/afUnboundData/data/submitterName**

Os dados do formulário adaptável são armazenados no elemento de dados, como mostrado acima. **_No XPath submitterName acima é o nome do campo de texto no Formulário adaptável._**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - Ao criar uma variável do tipo XML para capturar os dados enviados no modelo de fluxo de trabalho, não associe o XSD à variável. Isso ocorre porque quando você envia o Formulário adaptativo baseado em XSD os dados enviados não são compatíveis com o XSD. Os dados de reclamação do XSD são colocados no elemento /afData/afBoundData/ .
>
>**AEM Forms 6.5.1**  - Se você associar o XSD à variável XML, poderá navegar pelos elementos do esquema para fazer o mapeamento da variável. Não será possível acessar dados do formulário não vinculados a elementos do esquema. Se o caso de uso for acessar dados vinculados a elementos do esquema, bem como dados não vinculados, não vincule o esquema com sua variável XML no fluxo de trabalho. Você precisará usar a expressão XPath apropriada para obter os dados necessários

## Criando variáveis XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Uso do esquema com variável XML

**Mapeamento de uma variável XML com esquema. Use esse recurso com o AEM Forms 6.5.1 em diante**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Uso da variável no envio de email

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Para que os ativos funcionem em seu sistema, siga as seguintes etapas:

* [Baixe e importe os ativos no AEM usando o gerenciador de pacotes](assets/xmlandstringvariable.zip)
* [Explore o ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) modelo de fluxo de trabalho para entender as variáveis usadas no fluxo de trabalho
* [Configurar o serviço de email](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Preencha os detalhes e envie o formulário.

