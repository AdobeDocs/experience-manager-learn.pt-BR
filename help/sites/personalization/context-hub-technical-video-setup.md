---
title: Configurar o ContextHub para personalização com o AEM Sites
description: O ContextHub é uma estrutura para armazenar, manipular e apresentar dados de contexto. A API Javascript do ContextHub permite acessar armazenamentos para criar, atualizar e excluir dados, conforme necessário. Dessa forma, o ContextHub representa uma camada de dados nas páginas. Esta página descreve como adicionar um hub de contexto às páginas do site do AEM.
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 6%

---


# Configurar o ContextHub para personalização {#set-up-contexthub}

O ContextHub é uma estrutura para armazenar, manipular e apresentar dados de contexto. A API Javascript do ContextHub permite acessar armazenamentos para criar, atualizar e excluir dados, conforme necessário. Dessa forma, o ContextHub representa uma camada de dados nas páginas. Esta página descreve como adicionar um hub de contexto às páginas do site do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>Usamos o site de referência WKND para este vídeo e ele não faz parte da versão do AEM. Você pode baixar a [versão mais recente aqui](https://github.com/adobe/aem-guides-wknd/releases).

Adicione o ContextHub às suas páginas para ativar os recursos do ContextHub e para vincular às bibliotecas JavaScript do ContextHub. A API JavaScript do ContextHub fornece acesso aos dados de contexto que o ContextHub gerencia.

## Adicionar o ContextHub a um componente de página {#adding-contexthub-to-a-page-component}

Para ativar os recursos do ContextHub e vincular às bibliotecas JavaScript do ContextHub, inclua o componente `contexthub` na seção `<head>` da sua página da Web. O código HTL do componente de página se parece com o seguinte exemplo:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## Configuração do site e segmentos do ContextHub {#site-configuration-and-contexthub-segments}

O ContextHub inclui um mecanismo de segmentação que gerencia segmentos e determina quais segmentos são resolvidos para o contexto atual. Vários segmentos são definidos. Você pode usar a API do Javascript para [determinar os segmentos resolvidos](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). Ative os segmentos do ContextHub para seu site em [[!UICONTROL Navegador de configuração]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html).

## Criar segmentos {#create-segments}

Crie segmentos do AEM que atuam como regras para os teasers. Ou seja, eles definem quando o conteúdo em um teaser é exibido em uma página da Web. O conteúdo pode então ser direcionado especificamente para as necessidades e os interesses do visitante, dependendo dos segmentos aos quais ele corresponde.

## Atribuir configuração da nuvem, caminho do segmento e caminho do ContextHub ao seu site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Atribuir o caminho de configuração da nuvem, o caminho de segmentação e o caminho do ContextHub ao nó raiz do site para que você possa criar uma experiência personalizada para seu público-alvo. Usando o ContextHub, você pode manipular os dados de contexto e testar seus segmentos resolvidos.

![CRXDE Lite](assets/crx-de-properties.png)

Você pode ler mais sobre o ContextHub e a segmentação abaixo:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Adicionar o Context Hub à página e Acessar lojas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Noções sobre segmentação](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Configuração da segmentação com o ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
