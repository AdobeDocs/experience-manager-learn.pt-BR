---
title: Utilização da camada de dados do cliente Adobe com componentes principais AEM
description: A Camada de dados de clientes Adobe apresenta um método padrão para coletar e armazenar dados sobre a experiência de um visitante em uma página da Web e, em seguida, facilitar o acesso a esses dados. A Camada de dados de clientes Adobe é independente de plataforma, mas é totalmente integrada aos Componentes principais para uso com o AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
duration: 777
source-git-commit: dc40b8e022477d2b1d8f0ffe3b5e8bcf13be30b3
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 6%

---

# Utilização da camada de dados do cliente Adobe com componentes principais AEM {#overview}

A Camada de dados de clientes Adobe apresenta um método padrão para coletar e armazenar dados sobre a experiência de um visitante em uma página da Web e, em seguida, facilitar o acesso a esses dados. A Camada de dados de clientes Adobe é independente de plataforma, mas é totalmente integrada aos Componentes principais para uso com o AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Deseja ativar a Camada de dados de clientes Adobe no site AEM? [Veja as instruções aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR#installation-activation).

## Explorar a camada de dados

Você pode ter uma ideia da funcionalidade integrada da Camada de Dados do Cliente Adobe apenas usando as ferramentas de desenvolvedor do seu navegador e o [site de referência WKND](https://wknd.site/us/en.html) ativo.

>[!NOTE]
>
> Capturas de tela abaixo tiradas do navegador Chrome.

1. Navegue até [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra as ferramentas do desenvolvedor e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Para ver o estado atual da camada de dados em um site AEM, inspecione a resposta. Você deve ver informações sobre a página e os componentes individuais.

   ![Resposta da Camada de Dados Adobe](assets/data-layer-state-response.png)

1. Envie um objeto de dados para a camada de dados inserindo o seguinte no console:

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

   ![Retornar apenas uma única entrada de dados de componente](assets/return-just-single-component.png)

## Trabalhar com eventos

É uma prática recomendada acionar qualquer código personalizado com base em um evento da camada de dados. Em seguida, explore o registro e a escuta de diferentes eventos.

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

   O código acima inspeciona o objeto `event` e usa o método `adobeDataLayer.getState` para obter o estado atual do objeto que acionou o evento. O método auxiliar inspeciona o `filter` e somente se o `dataObject` atual atender aos critérios de filtro ele será retornado.

   >[!CAUTION]
   >
   > É importante **não** atualizar o navegador durante todo este exercício, caso contrário, o JavaScript do console será perdido.

1. Em seguida, insira um manipulador de eventos chamado quando um componente **Teaser** for exibido em um **Carrossel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/carousel/item"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   A função `teaserShownHandler` chama a função `getDataObjectHelper` e passa um filtro de `wknd/components/carousel/item` como `@type` para filtrar eventos acionados por outros componentes.

1. Em seguida, envie um ouvinte de eventos por push para a camada de dados para ouvir o evento `cmp:show`.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   O evento `cmp:show` é acionado por vários componentes diferentes, como quando um novo slide é mostrado no **Carrossel** ou quando uma nova guia é selecionada no componente **Guia**.

1. Na página, alterne os slides do carrossel e observe as instruções do console:

   ![Alternar Carrossel e ver ouvinte de eventos](assets/teaser-console-slides.png)

1. Para interromper a escuta do evento `cmp:show`, remova o ouvinte do evento da camada de dados

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Retorne à página e alterne os slides do carrossel. Observe que não há mais instruções registradas e que o evento não está sendo ouvido.

1. Em seguida, crie um manipulador de eventos que é chamado quando o evento de exibição de página é acionado:

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

1. Em seguida, envie um ouvinte de eventos por push para a camada de dados para ouvir o evento `cmp:show`, chamando o `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Você deve ver imediatamente uma instrução do console acionada com os dados da página:

   ![Dados de exibição de página](assets/page-show-console-data.png)

   O evento `cmp:show` para a página é acionado em cada carregamento de página na parte superior da página. Você pode perguntar, por que o manipulador de eventos foi acionado quando a página claramente já foi carregada?

   Um dos recursos exclusivos da Camada de Dados de Clientes Adobe é registrar ouvintes de eventos **antes** ou **depois** de a Camada de Dados ter sido inicializada. Isso ajuda a evitar as condições de corrida.

   A Camada de dados mantém uma matriz de filas de todos os eventos que ocorreram em sequência. A Camada de Dados por padrão disparará retornos de chamada de evento para eventos que ocorreram no **passado** e eventos no **futuro**. É possível filtrar os eventos do passado ou do futuro. [Mais informações podem ser encontradas na documentação](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Próximas etapas

Há duas opções para continuar aprendendo: primeiro, confira o tutorial [coletar dados de página e enviá-los para o Adobe Analytics](../analytics/collect-data-analytics.md) que demonstra o uso da camada de Dados de Clientes Adobe. A segunda opção é aprender a [Personalizar a Camada de Dados de Clientes Adobe com Componentes AEM](./data-layer-customize.md)


## Recursos adicionais {#additional-resources}

* [Documentação da Camada de Dados do Cliente do Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Usando a Camada de Dados do Cliente Adobe e a Documentação dos Componentes Principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR)
