---
title: Entrega de fragmentos de conteúdo no AEM
description: Os fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com Componentes principais ou podem ser entregues de maneira headless a canais downstream.
feature: Content Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 878
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---

# Entrega de fragmentos de conteúdo {#delivering-content-fragments}

Os fragmentos de conteúdo do Adobe Experience Manager (AEM) são conteúdos editoriais baseados em texto que podem incluir alguns elementos de dados estruturados associados, mas considerados conteúdo puro sem informações de design ou layout. Os fragmentos de conteúdo normalmente são criados como conteúdo independente de canal, que deve ser usado e reutilizado em canais, que por sua vez envolvem o conteúdo em uma experiência específica do contexto.

Os fragmentos de conteúdo, independentemente do layout, podem ser usados diretamente no AEM Sites com Componentes principais ou podem ser entregues de maneira headless a canais downstream.

Esta série de vídeos aborda as opções de entrega para usar Fragmentos de conteúdo. Detalhes sobre como definir e [criar fragmentos de conteúdo podem ser encontrados aqui](content-fragments-feature-video-use.md).

1. Uso de fragmentos de conteúdo em páginas da Web
2. Expor fragmentos de conteúdo como JSON usando os Serviços de conteúdo do AEM
3. Uso da API HTTP do Assets

## Uso de fragmentos de conteúdo em páginas da Web {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/34844?quality=12&learn=on&captions=por_br)

Os Fragmentos de conteúdo podem ser usados em páginas do AEM Sites ou, de maneira semelhante, em Fragmentos de experiência, usando o [componente de Fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR) do AEM WCM Core Components.

Os componentes do Fragmento de conteúdo podem ser estilizados usando o Sistema de estilos do AEM para exibir o conteúdo conforme necessário.

## Exposição de fragmentos de conteúdo como JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/34845?quality=12&learn=on&captions=por_br)

O AEM Content Services facilita a criação de pontos de extremidade HTTP baseados em página do AEM que renderizam conteúdo em um formato JSON normalizado.

O vídeo acima usa o [Componente de Fragmento de Conteúdo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR) para expor Fragmentos de Conteúdo individuais. O [Componente de Lista de Fragmentos de Conteúdo](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html?lang=pt-BR) é um novo componente que permite ao autor definir uma consulta que preencherá dinamicamente a página com uma lista de Fragmentos de Conteúdo. O componente de Lista de fragmentos de conteúdo é preferido quando vários fragmentos de conteúdo precisam ser expostos.

*Exemplo de carga JSON do ponto de extremidade dos Serviços de Conteúdo:*\
**[atletas.json](assets/athletes.json)**

## Uso da API HTTP do Assets

>[!VIDEO](https://video.tv.adobe.com/v/41128?quality=12&learn=on&captions=por_br)

Introduzido pela primeira vez no AEM 6.5, é o suporte aprimorado para Fragmentos de conteúdo com a API HTTP do Assets. Isso oferece uma maneira fácil para os desenvolvedores executarem operações de Criar, Ler, Atualizar e Excluir (CRUD) em relação aos Fragmentos de conteúdo.

*Exemplo de Solicitações do POSTMAN:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Qual método de entrega usar

### Canal da Web

A abordagem para fornecer um fragmento de conteúdo por meio de um canal da Web é simples, pois usa o componente Fragmento de conteúdo com o AEM Sites.

### Headless

Há duas opções para expor o Fragmento de conteúdo como JSON para oferecer suporte a um canal de terceiros em um caso de uso headless:

1. Use as páginas AEM Content Services e API de proxy (Vídeo #2) quando o caso de uso principal for fornecer Fragmentos de conteúdo para consumo (somente leitura) por um canal de terceiros. A estrutura do Content Services oferece mais flexibilidade e opções sobre quais dados são expostos. Os desenvolvedores também podem estender a estrutura dos Serviços de conteúdo para aumentar e/ou enriquecer os dados.

2. Use a API HTTP do Assets (Vídeo #3) quando o canal de terceiros precisar modificar e/ou atualizar fragmentos de conteúdo. Um caso de uso típico é a assimilação de conteúdo de terceiros em um ambiente de autor do AEM.

## Recursos adicionais {#additional-resources}

* [Criação de fragmentos de conteúdo](content-fragments-feature-video-use.md)
* [Componentes principais de WCM do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Componente WCM do fragmento de conteúdo principal do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR)

Para baixar e instalar o pacote abaixo em uma instância do AEM 6.4+, para o estado final da série de vídeos:\
**[aem_demo_flow-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
