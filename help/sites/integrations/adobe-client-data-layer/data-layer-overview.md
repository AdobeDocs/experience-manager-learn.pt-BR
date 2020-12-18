---
title: Uso da camada de dados do cliente Adobe com componentes principais AEM
description: A Camada de dados do cliente Adobe apresenta um método padrão para coletar e armazenar dados sobre uma experiência de visitantes em uma página da Web e, em seguida, facilitar o acesso a esses dados. A Adobe Client Data Layer é agnóstica de plataforma, mas é totalmente integrada aos Componentes principais para uso com o AEM.
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: aa48c94413f83e794c5d062daaac85c97b451b82
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 1%

---


# Usando a camada de dados do cliente Adobe com AEM componentes principais {#overview}

A Camada de dados do cliente Adobe apresenta um método padrão para coletar e armazenar dados sobre uma experiência de visitantes em uma página da Web e, em seguida, facilitar o acesso a esses dados. A Adobe Client Data Layer é agnóstica de plataforma, mas é totalmente integrada aos Componentes principais para uso com o AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Deseja habilitar a Camada de Dados do Cliente Adobe no seu site AEM? [Veja as instruções aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Explore a camada de dados

Você pode obter uma ideia da funcionalidade integrada da Camada de Dados do Cliente Adobe usando apenas as ferramentas do desenvolvedor do seu navegador e o [site de referência WKND](https://wknd.site/) ao vivo.

>[!NOTE]
>
> Capturas de tela abaixo tiradas do navegador Chrome.

1. Navegue até [https://wknd.site](https://wknd.site)
1. Abra as ferramentas do desenvolvedor e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect a resposta para ver o estado atual da camada de dados em um site AEM. Você deve ver informações sobre a página e os componentes individuais.

   ![Resposta da camada de dados de Adobe](assets/data-layer-state-response.png)

1. Empurre um objeto de dados para a camada de dados inserindo o seguinte no console:

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. Execute o comando `adobeDataLayer.getState()` novamente e localize a entrada para `training-data`.
1. Em seguida, adicione um parâmetro de caminho para retornar apenas o estado específico de um componente:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Retornar apenas uma entrada de dados de componente](assets/return-just-single-component.png)

## Trabalhar com Eventos

É uma prática recomendada acionar qualquer código personalizado com base em um evento da camada de dados. Em seguida, explore o registro e o acompanhamento de eventos diferentes.

1. Digite o seguinte método auxiliar no console:

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   O código acima inspecionará o objeto `event` e usará o método `adobeDataLayer.getState` para obter o estado atual do objeto que disparou o evento. O método auxiliar inspecionará os critérios `filter` e somente se o `dataObject` atual atender ao filtro será retornado.

   >[!CAUTION]
   >
   > Será importante **e não** atualizar o navegador durante todo este exercício, caso contrário o JavaScript do console será perdido.

1. Em seguida, digite um manipulador de eventos que será chamado quando um componente **Teaser** for exibido em um **carrossel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   O `teaserShownHandler` chamará o método `getDataObjectHelper` e passará em um filtro de `wknd/components/teaser` como o `@type` para filtrar eventos acionados por outros componentes.

1. Em seguida, empurre um ouvinte de evento na camada de dados para ouvir o evento `cmp:show`.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   O evento `cmp:show` é acionado por muitos componentes diferentes, como quando um novo slide é mostrado no **carrossel** ou quando uma nova guia é selecionada no componente **Tab**.

1. Na página, alterne os slides do carrossel e observe as instruções do console:

   ![Alternar entre o carrossel e ver o ouvinte do evento](assets/teaser-console-slides.png)

1. Remova o ouvinte de evento da camada de dados para parar de acompanhar o evento `cmp:show`:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Retorne à página e alterne os slides do carrossel. Observe que não há mais declarações registradas e que o evento não está sendo escutado.

1. Em seguida, digite um manipulador de eventos que será chamado quando o evento exibido na página for acionado:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Observe que o tipo de recurso `wknd/components/page` é usado para filtrar o evento.

1. Em seguida, empurre um ouvinte de evento na camada de dados para ouvir o evento `cmp:show`, chamando `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Você deve ver imediatamente uma declaração do console acionada com os dados da página:

   ![Mostrar dados da página](assets/page-show-console-data.png)

   O evento `cmp:show` da página é acionado em cada carregamento de página na parte superior da página. Você pode perguntar, por que o manipulador de eventos foi acionado, quando a página claramente já foi carregada?

   Este é um dos recursos exclusivos da Camada de dados do cliente Adobe, na medida em que você pode registrar ouvintes de evento **antes de** ou **depois de** a Camada de dados ter sido inicializada. Esta é uma característica crítica para evitar condições raciais.

   A Camada de dados mantém uma matriz de fila de todos os eventos que ocorreram em sequência. Por padrão, a Camada de Dados acionará retornos de chamada de evento para eventos que ocorreram no **previous**, bem como eventos no **future**. É possível filtrar os eventos para apenas passado ou futuro. [Para obter mais informações, consulte a documentação](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Próximas etapas

Consulte o tutorial a seguir para saber como usar a camada de Dados do cliente Adobe orientada por eventos para [coletar dados de página e enviar para o Adobe Analytics](../analytics/collect-data-analytics.md).

Ou saiba como [Personalizar a camada de dados do cliente Adobe com componentes AEM](./data-layer-customize.md)


## Recursos adicionais {#additional-resources}

* [Documentação da camada de dados do cliente Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Uso da camada de dados do cliente Adobe e da documentação dos componentes principais](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
