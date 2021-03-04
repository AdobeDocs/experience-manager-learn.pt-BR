---
title: Tutoriais do AEM Headless
description: Uma coleção de tutoriais sobre como usar o Adobe Experience Manager as a Headless CMS.
feature: Fragmentos de conteúdo, APIs
topic: Sem periféricos, gerenciamento de conteúdo
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 4%

---


# Tutoriais do AEM Headless

O Adobe Experience Manager tem várias opções para definir endpoints sem periféricos e fornecer seu conteúdo como JSON. Use tutoriais práticos para explorar como usar as várias opções e escolher o que é certo para você.

## Tutorial de APIs GraphQL do AEM

>[!CAUTION]
>
> A API GraphQL do AEM para entrega de fragmentos de conteúdo está disponível mediante solicitação.
> Entre em contato com o Suporte da Adobe para ativar a API do seu programa AEM as a Cloud Service.

APIs GraphQL do AEM para fragmentos de conteúdo
O suporta cenários CMS sem periféricos, onde aplicativos cliente externos renderizam experiências usando conteúdo gerenciado no AEM.

Uma API moderna de entrega de conteúdo é fundamental para a eficiência e o desempenho de aplicativos de front-end baseados em Javascript. Usar uma REST API apresenta desafios:

* Grande número de solicitações para buscar um objeto de cada vez
* Com frequência, o conteúdo é &quot;excessivamente fornecido&quot;, o que significa que o aplicativo recebe mais do que precisa

Para superar esses desafios, o GraphQL fornece uma API baseada em consulta que permite que os clientes consultem o AEM somente para o conteúdo necessário e recebam usando uma única chamada de API.

* Saiba como usar as APIs GraphQL do AEM no tutorial [Introdução às APIs GraphQL do AEM](./graphql/overview.md)

## Tutorial de autenticação baseado em token

O AEM expõe uma variedade de pontos de extremidade HTTP que podem ter interação headless, de GraphQL, AEM Content Services para API HTTP do Assets. Geralmente, esses consumidores sem interface podem precisar se autenticar no AEM para acessar o conteúdo ou as ações protegidas. Para facilitar isso, o AEM oferece suporte à autenticação por token de solicitações HTTP de aplicativos, serviços ou sistemas externos.

* Saiba como autenticar no AEM por HTTP usando tokens de acesso no [Autenticar para o AEM as a Cloud Service a partir de um tutorial de aplicativo externo](./authentication/overview.md)

## Tutorial do AEM Content Services

Os Serviços de conteúdo do AEM usam as páginas tradicionais do AEM para compor pontos de extremidade da API REST sem periféricos e os Componentes do AEM definem ou fazem referência ao conteúdo a ser exposto nesses pontos de extremidade.

Os AEM Content Services permitem as mesmas abstrações de conteúdo usadas para criar páginas da Web no AEM Sites, para definir o conteúdo e os esquemas dessas APIs HTTP. O uso de Páginas do AEM e Componentes do AEM capacita os profissionais de marketing a compor e atualizar rapidamente APIs JSON flexíveis que podem alimentar qualquer aplicativo.

* Saiba como usar os Serviços de conteúdo do AEM no tutorial [Introdução aos serviços de conteúdo do AEM](./content-services/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | APIs GraphQL do AEM | Serviços de conteúdo AEM |
|--------------------------------|:-----------------|:---------------------|
| Definição de esquema | Modelos de fragmentos de conteúdo estruturados | Componentes do AEM |
| Conteúdo | Fragmentos de conteúdo | Componentes do AEM |
| Descoberta de conteúdo | Por consulta GraphQL | Por página do AEM |
| Formato de entrega | GraphQL JSON | AEM ComponentExporter JSON |

## Outros tutoriais úteis

Outros tutoriais do AEM relacionados a conceitos sem cabeçalho incluem:

* [Introdução ao Editor e Angular de SPA do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Introdução ao Editor de SPA no AEM e React](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)