---
title: Fornecer fragmentos de conteúdo em AEM
seo-title: Fornecer fragmentos de conteúdo no Adobe Experience Manager
description: Os Fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com os Componentes principais ou podem ser fornecidos de maneira sem cabeçalho a canais posteriores.
seo-description: Os Fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com os Componentes principais ou podem ser fornecidos de maneira sem cabeçalho a canais posteriores.
sub-product: serviços de conteúdo
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 6%

---


# Fornecer fragmentos de conteúdo {#delivering-content-fragments}

Fragmentos de conteúdo da Adobe Experience Manager (AEM) são conteúdos editoriais baseados em texto que podem incluir alguns elementos de dados estruturados associados, mas considerados conteúdo puro sem informações de design ou layout. Os Fragmentos de conteúdo normalmente são criados como conteúdo agnóstico do canal, que deve ser usado e reutilizado em canais, o que, por sua vez, envolve o conteúdo em uma experiência específica do contexto.

Os Fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com os Componentes principais ou podem ser fornecidos de maneira sem cabeçalho a canais posteriores.

Esta série de vídeo aborda as opções de delivery para usar Fragmentos de conteúdo. Detalhes sobre como definir e [criar Fragmentos de conteúdo podem ser encontrados aqui](content-fragments-feature-video-use.md).

1. Uso de fragmentos de conteúdo em páginas da Web
2. Exposição de fragmentos de conteúdo como JSON usando AEM Content Services
3. Uso da API HTTP Assets

## Uso de fragmentos de conteúdo em páginas da Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Fragmentos de conteúdo podem ser usados em páginas do AEM Sites ou de maneira semelhante, em Fragmentos de experiência, usando o componente [Fragmento de](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/components/content-fragment-component.html)conteúdo dos Componentes principais do AEM WCM.

Os componentes do Fragmento de conteúdo podem ser estilizados usando AEM Sistema de estilo para exibir o conteúdo conforme necessário.

## Exposição de fragmentos de conteúdo como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services facilita a criação de pontos finais HTTP baseados em AEM página que renderizam o conteúdo em um formato JSON normalizado.

O vídeo acima usa o Componente [de fragmento de](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/components/content-fragment-component.html) conteúdo para expor Fragmentos de conteúdo individuais. O Componente [de Lista de fragmento de](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) conteúdo é um novo componente que permite que um autor defina um query que preencherá dinamicamente a página com uma lista de Fragmentos de conteúdo. O componente de Lista do fragmento do conteúdo é preferido quando vários Fragmentos do conteúdo precisam ser expostos.

*Exemplo de carga JSON de ponto final do Content Services:*\
**[athletes.json](assets/athletes.json)**

## Uso da API HTTP Assets

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

A primeira introdução no AEM 6.5 é o suporte aprimorado para Fragmentos de conteúdo com a API HTTP Assets. Isso fornece uma maneira fácil para os desenvolvedores executarem operações de Criar, Ler, Atualizar e Excluir (CRUD) em Fragmentos de conteúdo.

*Exemplo de solicitações POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Que método de delivery usar

### Canal da Web

A abordagem para fornecer um Fragmento de conteúdo por meio de um canal da Web é simples ao usar o componente Fragmento de conteúdo com a AEM Sites.

### Sem cabeça

Há duas opções para expor o Fragmento de conteúdo como JSON para suportar um canal de terceiros em um caso de uso sem cabeçalho:

1. Use as páginas AEM Content Services e Proxy API (Vídeo nº 2) quando o caso de uso principal for fornecer Fragmentos de conteúdo para consumo (Somente leitura) por um canal de terceiros. A estrutura do Content Services fornece mais flexibilidade e opções sobre quais dados são expostos. Os desenvolvedores também podem estender a estrutura do Content Services para aumentar e/ou enriquecer os dados.

2. Use a API HTTP Ativos (Vídeo nº 3) quando o canal de terceiros precisar modificar e/ou atualizar Fragmentos de conteúdo. Um caso de uso típico é a assimilação de conteúdo de terceiros em um ambiente de autor AEM.

## Recursos adicionais {#additional-resources}

* [Criação de fragmentos de conteúdo](content-fragments-feature-video-use.md)
* [Componentes principais do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Componente de fragmento de conteúdo principal do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/components/content-fragment-component.html)

Para baixar e instalar o pacote abaixo em uma instância AEM 6.4+ para o estado final da série de vídeo:\
**[aem_demo_fluidescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
