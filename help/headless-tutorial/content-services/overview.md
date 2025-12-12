---
title: 'Introdução ao AEM Headless: serviços de conteúdo'
description: Um tutorial completo que ilustra como criar e expor conteúdo usando o AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# Introdução ao AEM Headless: serviços de conteúdo

Os serviços de conteúdo do AEM utilizam as páginas tradicionais do AEM para compor pontos de acesso da API REST sem cabeçalho, e os componentes do AEM definem ou fazem referência ao conteúdo a ser exposto nesses pontos de acesso.

Os serviços de conteúdo do AEM permitem as mesmas abstrações de conteúdo usadas para a criação de páginas da web no AEM Sites, a fim de definir o conteúdo e os esquemas dessas APIs HTTP. O uso de páginas e componentes do AEM permite que os profissionais de marketing criem e atualizem rapidamente APIs JSON flexíveis e capazes de impulsionar qualquer aplicativo.

## Tutorial dos serviços de conteúdo

Um tutorial completo que ilustra como criar e expor conteúdo com o AEM, e consumi-lo por meio de um aplicativo móvel nativo, em um cenário sem cabeçalho do CMS.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

Este tutorial explora como os serviços de conteúdo do AEM podem ser usados para impulsionar a experiência de um aplicativo móvel que exibe informações de eventos (música, shows, arte etc.) com curadoria da equipe da WKND.

Este tutorial abordará os seguintes tópicos:

* Criar conteúdo que represente um evento por meio de fragmentos de conteúdo
* Definir um ponto de acesso dos serviços de conteúdo do AEM, usando modelos e páginas do AEM Sites que exponham os dados dos eventos como JSON
* Saiba como os componentes principais do AEM WCM podem ser usados para permitir que os profissionais de marketing criem pontos de acesso JSON
* Consumir o JSON dos serviços de conteúdo do AEM a partir de um aplicativo móvel
   * O uso de um sistema Android deve-se ao fato de ele ter um emulador multiplataforma que todos os usuários (Windows, macOS e Linux) deste tutorial podem usar para executar o aplicativo nativo.

## Projeto do GitHub

O código-fonte e os pacotes de conteúdo estão disponíveis em [Guias do AEM: projeto do GitHub da WKND para dispositivos móveis](https://github.com/adobe/aem-guides-wknd-mobile).

Se você encontrar um problema no tutorial ou no código, crie um [problema no GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## GraphQL do AEM x Serviços de conteúdo do AEM

|                                | APIs GraphQL do AEM | Serviços de conteúdo do AEM |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmento de conteúdo estruturado | Componentes do AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes do AEM |
| Descoberta de conteúdo | Por consulta do GraphQL | Por página do AEM |
| Formato de entrega | GraphQL JSON | AEM ComponentExporter JSON |
