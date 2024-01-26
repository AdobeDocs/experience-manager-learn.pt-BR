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
duration: 818
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 6%

---

# Utilização da camada de dados do cliente Adobe com componentes principais AEM {#overview}

A Camada de dados de clientes Adobe apresenta um método padrão para coletar e armazenar dados sobre a experiência de um visitante em uma página da Web e, em seguida, facilitar o acesso a esses dados. A Camada de dados de clientes Adobe é independente de plataforma, mas é totalmente integrada aos Componentes principais para uso com o AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Deseja ativar a Camada de dados de clientes Adobe no site AEM? [Veja as instruções aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Explorar a camada de dados

Você pode ter uma ideia da funcionalidade integrada da Camada de dados do cliente Adobe apenas usando as ferramentas de desenvolvedor do seu navegador e o Live [Site de referência da WKND](https://wknd.site/us/en.html).

>[!NOTE]
>
> Capturas de tela abaixo tiradas do navegador Chrome.

1. Navegue até [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra as ferramentas do desenvolvedor e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Para ver o estado atual da camada de dados em um site AEM, inspecione a resposta. Você deve ver informações sobre a página e os componentes individuais.

   ![Resposta da camada de dados Adobe](assets/data-layer-state-response.png)

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

   ![Retorna apenas uma única entrada de dados de componente](assets/return-just-single-component.png)

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

   O código acima inspeciona o `event` objeto e usa o `adobeDataLayer.getState` para obter o estado atual do objeto que acionou o evento. Em seguida, o método auxiliar inspeciona o `filter` e somente se o atual `dataObject` atende aos critérios de filtro retornados.

   >[!CAUTION]
   >
   > É importante **não** para atualizar o navegador durante todo este exercício, caso contrário, o JavaScript do console será perdido.

1. Em seguida, insira um manipulador de eventos chamado quando um **Teaser** componente é exibido em um **Carrossel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   A variável `teaserShownHandler` chama a função `getDataObjectHelper` e passa um filtro de `wknd/components/teaser` como o `@type` para filtrar eventos acionados por outros componentes.

1. Em seguida, envie um ouvinte de eventos para a camada de dados para ouvir o `cmp:show` evento.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   A variável `cmp:show` evento é acionado por vários componentes diferentes, como quando um novo slide é mostrado no **Carrossel** ou quando uma nova guia é selecionada na variável **Guia** componente.

1. Na página, alterne os slides do carrossel e observe as instruções do console:

   ![Alterne o Carrossel e consulte o ouvinte de eventos](assets/teaser-console-slides.png)

1. Para parar de ouvir o `cmp:show` evento, remova o ouvinte de eventos da camada de dados

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

1. Em seguida, envie um ouvinte de eventos para a camada de dados para ouvir o `cmp:show` evento, chamar o `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. Você deve ver imediatamente uma instrução do console acionada com os dados da página:

   ![Dados de exibição de página](assets/page-show-console-data.png)

   A variável `cmp:show` para a página é acionado em cada carregamento de página na parte superior da página. Você pode perguntar, por que o manipulador de eventos foi acionado quando a página claramente já foi carregada?

   Um dos recursos exclusivos da Camada de dados de clientes Adobe é que você pode registrar ouvintes de eventos **antes** ou **após** A Camada de dados foi inicializada e ajuda a evitar as condições de corrida.

   A Camada de dados mantém uma matriz de filas de todos os eventos que ocorreram em sequência. A Camada de dados, por padrão, acionará retornos de chamada de evento para eventos que ocorreram na **passado** e eventos na **futuro**. É possível filtrar os eventos do passado ou do futuro. [Mais informações podem ser encontradas na documentação](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Próximas etapas

Há duas opções para continuar aprendendo: primeiro, confira o [coletar dados de página e enviá-los para a Adobe Analytics](../analytics/collect-data-analytics.md) tutorial que demonstra o uso da camada de dados do cliente Adobe. A segunda opção é aprender a [Personalizar a Camada de dados do cliente Adobe com componentes AEM](./data-layer-customize.md)


## Recursos adicionais {#additional-resources}

* [Documentação da Camada de dados do cliente Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Uso da Camada de dados do cliente Adobe e da Documentação dos Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR)
