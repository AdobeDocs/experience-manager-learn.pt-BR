---
title: AEM Tutoriais sem cabeçalho
description: Uma coleção de tutoriais sobre como usar o Adobe Experience Manager como um CMS sem cabeçalho.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 5%

---


# AEM Tutoriais sem cabeçalho

A Adobe Experience Manager tem várias opções para definir pontos de extremidade sem cabeçalho e fornecer seu conteúdo como JSON. Use tutoriais práticos para explorar como usar as várias opções e escolher o que é certo para você.

## Tutorial de APIs do AEM GraphQL

>[!CAUTION]
>
> A API AEM GraphQL para o Delivery de fragmento de conteúdo será lançada no início de 2021.
> A documentação relacionada está disponível para fins de pré-visualização.

AEM APIs GraphQL para fragmentos de conteúdo
oferece suporte a cenários CMS sem cabeçalho onde aplicativos cliente externos renderizam experiências usando conteúdo gerenciado em AEM.

Uma moderna API de delivery de conteúdo é fundamental para a eficiência e o desempenho de aplicativos de front-end baseados em Javascript. Usar uma REST API apresenta desafios:

* Grande número de solicitações para buscar um objeto por vez
* Com frequência, o conteúdo &quot;sobrealimentado&quot; significa que o aplicativo recebe mais do que precisa

Para superar esses desafios, o GraphQL fornece uma API baseada em query que permite que os clientes query AEM somente o conteúdo necessário e que recebam usando uma única chamada de API.

* Saiba como usar AEM APIs GraphQL utilizando o tutorial [Introdução AEM APIs GraphQL](./graphql/overview.md)

## Tutorial do AEM Content Services

AEM Content Services aproveita as páginas AEM tradicionais para compor pontos finais de API REST sem cabeçalho e AEM Os componentes definem, ou fazem referência, ao conteúdo a ser exposto nesses pontos finais.

AEM Content Services permite as mesmas abstrações de conteúdo usadas para criar páginas da Web no AEM Sites, para definir o conteúdo e os schemas dessas APIs HTTP. O uso de Páginas AEM e Componentes AEM permite que os profissionais de marketing componham e atualizem rapidamente APIs JSON flexíveis que podem acionar qualquer aplicativo.

* Saiba como usar AEM Content Services no tutorial [Introdução ao AEM Content Services](./content-services/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | AEM APIs GraphQL | Serviços de conteúdo AEM |
|--------------------------------|:-----------------|:---------------------|
| Definição de schema | Modelos de fragmento de conteúdo estruturado | Componentes AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes AEM |
| Detecção de conteúdo | Por query GraphQL | Por AEM página |
| Formato do delivery | GraphQL JSON | AEM ComponentExporter JSON |

## Outros tutoriais úteis

Outros tutoriais AEM pertencendo aos conceitos sem cabeçalho incluem:

* [Introdução ao Editor e Angular de SPA do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Introdução ao Editor de SPA no AEM e React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)