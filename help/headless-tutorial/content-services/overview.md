---
title: Introdução ao AEM Headless - Serviços de conteúdo
description: Um tutorial completo que ilustra como criar e expor conteúdo usando o AEM Headless.
feature: Fragmentos de conteúdo, APIs
topic: Sem periféricos, gerenciamento de conteúdo
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 4%

---


# Introdução ao AEM Headless - Serviços de conteúdo

Os AEM Content Services usam as AEM Pages tradicionais para compor endpoints de API REST sem periféricos e AEM Componentes definem, ou fazem referência, ao conteúdo a ser exposto nesses endpoints.

Os AEM Content Services permitem as mesmas abstrações de conteúdo usadas para criar páginas da Web no AEM Sites, para definir o conteúdo e os esquemas dessas APIs HTTP. O uso de AEM Páginas e Componentes AEM capacita os profissionais de marketing a compor e atualizar rapidamente APIs JSON flexíveis que podem acionar qualquer aplicativo.

## Tutorial dos serviços de conteúdo

Um tutorial completo que ilustra como criar e expor conteúdo usando AEM e consumido por um aplicativo móvel nativo, em um cenário de CMS sem periféricos.

>[!VIDEO](https://video.tv.adobe.com/v/28315/?quality=12&learn=on)

Este tutorial explica como AEM Content Services pode ser usado para potencializar a experiência de um aplicativo móvel que exibe informações do evento (música, desempenho, arte, etc.) que é preparado pela equipe da WKND.

Este tutorial abordará os seguintes tópicos:

* Criar conteúdo que represente um evento usando Fragmentos de conteúdo
* Defina um ponto final dos Serviços de conteúdo de AEM usando Modelos e páginas do AEM Sites que expõem os dados do Evento como JSON
* Explore como AEM os Componentes principais do WCM podem ser usados para permitir que os profissionais de marketing criem pontos finais JSON
* Consumir AEM Content Services JSON de um aplicativo móvel
   * O uso do Android é porque ele tem um emulador de plataforma cruzada que todos os usuários (Windows, macOS e Linux) deste tutorial podem usar para executar o aplicativo nativo.

## Projeto do GitHub

O código-fonte e os pacotes de conteúdo estão disponíveis nos [Guias AEM - Projeto GitHub Móvel WKND](https://github.com/adobe/aem-guides-wknd-mobile).

Se você encontrar um problema com o tutorial ou o código, deixe um [problema do GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL vs. AEM Content Services

|  | AEM APIs GraphQL | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmentos de conteúdo estruturados | Componentes AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes AEM |
| Descoberta de conteúdo | Por consulta GraphQL | Por AEM página |
| Formato de entrega | GraphQL JSON | AEM ComponentExporter JSON |
