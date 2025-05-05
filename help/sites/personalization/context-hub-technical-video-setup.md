---
title: Configuração do ContextHub para Personalization com AEM Sites
description: O ContextHub é uma estrutura para armazenar, manipular e apresentar dados de contexto. A API do Javascript do ContextHub permite acessar armazenamentos para criar, atualizar e excluir dados conforme necessário. Dessa forma, o ContextHub representa uma camada de dados em suas páginas. Esta página descreve como adicionar o hub de contexto às páginas do site do AEM.
feature: Context Hub
version: Experience Manager 6.4, Experience Manager 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 357
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 2%

---

# Configurar o ContextHub para o Personalization {#set-up-contexthub}

O ContextHub é uma estrutura para armazenar, manipular e apresentar dados de contexto. A API do Javascript do ContextHub permite acessar armazenamentos para criar, atualizar e excluir dados conforme necessário. Dessa forma, o ContextHub representa uma camada de dados em suas páginas. Esta página descreve como adicionar o hub de contexto às páginas do site do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/34853?quality=12&learn=on&captions=por_br)

>[!NOTE]
>
>Usamos o site de referência da WKND para este vídeo e ele não faz parte da versão do AEM. Você pode baixar a [última versão aqui](https://github.com/adobe/aem-guides-wknd/releases).

Adicione o ContextHub às suas páginas para ativar os recursos do ContextHub e para vincular às bibliotecas de JavaScript do ContextHub. A API do ContextHub JavaScript fornece acesso aos dados de contexto que o ContextHub gerencia.

## Adicionar o ContextHub a um componente de Página {#adding-contexthub-to-a-page-component}

Para habilitar os recursos do ContextHub e vincular às bibliotecas JavaScript do ContextHub, inclua o componente `contexthub` na seção `<head>` da sua página da Web. O código HTL do seu componente Página é semelhante ao seguinte exemplo:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## Configuração do site e segmentos do ContextHub {#site-configuration-and-contexthub-segments}

O ContextHub inclui um mecanismo de segmentação que gerencia segmentos e determina quais segmentos são resolvidos para o contexto atual. Vários segmentos estão definidos. Você pode usar a API do Javascript para [determinar segmentos resolvidos](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Habilite os segmentos do ContextHub para seu site em [[!UICONTROL Navegador de Configuração]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR).

## Criar segmentos {#create-segments}

Crie segmentos do AEM que atuam como regras para os teasers. Ou seja, eles definem quando o conteúdo de um teaser aparece em uma página da Web. O conteúdo pode então ser direcionado especificamente para as necessidades e os interesses do visitante, dependendo dos segmentos aos quais ele corresponde.

## Atribuindo a configuração de nuvem, o caminho do segmento e o caminho do ContextHub ao site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Atribuir o caminho de configuração da nuvem, o caminho de segmentação e o caminho do ContextHub ao nó raiz do site para que você possa criar uma experiência personalizada para o público-alvo. Usando o ContextHub, você pode manipular os dados de contexto e testar os segmentos resolvidos.

![CRXDE Lite](assets/crx-de-properties.png)

Você pode ler mais sobre o ContextHub e a segmentação abaixo:

* [ContextHub](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Adicionando o Context Hub à página e Acessando Lojas](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Noções sobre segmentação](https://helpx.adobe.com/br/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configuração de segmentação com o ContextHub](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/segmentation.html)
