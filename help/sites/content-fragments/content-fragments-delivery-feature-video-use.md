---
title: Entrega de fragmentos de conteúdo em AEM
seo-title: Entrega de fragmentos de conteúdo no Adobe Experience Manager
description: Os Fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com os Componentes principais ou podem ser entregues de maneira headless a canais de downstream.
seo-description: Os Fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com os Componentes principais ou podem ser entregues de maneira headless a canais de downstream.
sub-product: serviços de conteúdo
feature: Fragmentos de conteúdo
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Gerenciamento de conteúdo
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---


# Entrega de fragmentos de conteúdo {#delivering-content-fragments}

Os Fragmentos de conteúdo do Adobe Experience Manager (AEM) são conteúdos editoriais baseados em texto que podem incluir alguns elementos de dados estruturados associados, mas considerados conteúdo puro sem informações de design ou layout. Os Fragmentos de conteúdo geralmente são criados como conteúdo independente de canal, que deve ser usado e reutilizado em canais, o que, por sua vez, envolve o conteúdo em uma experiência específica de contexto.

Os Fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com os Componentes principais ou podem ser entregues de maneira headless a canais de downstream.

Esta série de vídeo aborda as opções de entrega para usar Fragmentos de conteúdo. Detalhes sobre como definir e [criar Fragmentos de conteúdo podem ser encontrados aqui](content-fragments-feature-video-use.md).

1. Uso de fragmentos de conteúdo em páginas da Web
2. Exposição de fragmentos de conteúdo como JSON usando AEM Content Services
3. Uso da API HTTP de ativos

## Uso de fragmentos de conteúdo em páginas da Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Os Fragmentos de conteúdo podem ser usados nas páginas do AEM Sites ou de maneira semelhante, nos Fragmentos de experiência, usando o componente do Fragmento de conteúdo dos Componentes principais do WCM AEM [Componente do fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html).

Os componentes do Fragmento de conteúdo podem ser estilizados usando AEM Sistema de estilos para exibir o conteúdo conforme necessário.

## Exposição de fragmentos de conteúdo como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilita a criação de AEM pontos finais HTTP baseados em página, que renderizam o conteúdo em um formato JSON normalizado.

O vídeo acima usa o [Componente do fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) para expor Fragmentos de conteúdo individuais. O [Componente da lista de fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) é um novo componente que permite que um autor defina uma consulta que preencherá dinamicamente a página com uma lista de Fragmentos de conteúdo. O componente Lista de fragmentos do conteúdo é preferido quando vários Fragmentos do conteúdo precisam ser expostos.

*Exemplo de carga JSON de ponto final dos Serviços de conteúdo:*\
**[athletes.json](assets/athletes.json)**

## Uso da API HTTP de ativos

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Primeiramente introduzido no AEM 6.5, o é um suporte aprimorado para Fragmentos de conteúdo com a API HTTP do Assets. Isso fornece uma maneira fácil para os desenvolvedores executarem operações de Criar, Ler, Atualizar e Excluir (CRUD) em relação aos Fragmentos de conteúdo.

*Exemplo de solicitações POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Qual método de delivery usar

### Canal da Web

A abordagem para fornecer um Fragmento de conteúdo por meio de um canal da Web é simples usando o componente Fragmento de conteúdo com o AEM Sites.

### Cabeça

Há duas opções para expor o Fragmento de conteúdo como JSON para suportar um canal de terceiros em um caso de uso sem interface:

1. Use AEM páginas Serviços de conteúdo e API de proxy (Vídeo nº 2) quando o caso de uso principal for fornecer Fragmentos de conteúdo para consumo (somente leitura) por um canal de terceiros. A estrutura dos Serviços de conteúdo oferece mais flexibilidade e opções sobre quais dados são expostos. Os desenvolvedores também podem estender a estrutura dos Serviços de conteúdo para aumentar e/ou enriquecer os dados.

2. Use a API HTTP de ativos (vídeo nº 3) quando o canal de terceiros precisar modificar e/ou atualizar os Fragmentos de conteúdo. Um caso de uso típico é a assimilação de conteúdo de terceiros em um ambiente de criação de AEM.

## Recursos adicionais {#additional-resources}

* [Criação de fragmentos de conteúdo](content-fragments-feature-video-use.md)
* [Componentes principais do WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Componente do Fragmento de conteúdo principal do WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

Para baixar e instalar o pacote abaixo em uma instância do AEM 6.4+ para o estado final da série de vídeos:\
**[aem_demo_fluidoexperiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
