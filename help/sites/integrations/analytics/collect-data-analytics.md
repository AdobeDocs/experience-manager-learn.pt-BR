---
title: Coletar dados de página com o Adobe Analytics
description: Use a camada Dados do cliente Adobe orientada por evento para coletar dados sobre a atividade do usuário em um site criado com a Adobe Experience Manager. Saiba como usar regras no Experience Platform Launch para acompanhar esses eventos e enviar dados para um conjunto de relatórios da Adobe Analytics.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
translation-type: tm+mt
source-git-commit: 096cdccdf1675480aa0a35d46ce7b62a3906dad1
workflow-type: tm+mt
source-wordcount: '2414'
ht-degree: 2%

---


# Coletar dados de página com o Adobe Analytics

Saiba como usar os recursos integrados da Camada de dados do cliente [Adobe com AEM componentes](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/data-layer/overview.html) principais para coletar dados sobre uma página no Adobe Experience Manager Sites. [O Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) e a extensão [do](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) Adobe Analytics serão usados para criar regras para enviar dados de página à Adobe Analytics.

## O que você vai criar

![Rastreamento de dados da página](assets/collect-data-analytics/analytics-page-data-tracking.png)

Neste tutorial, você acionará uma regra de inicialização com base em um evento da camada de dados do cliente Adobe, adicionará condições para quando a regra deve ser acionada e enviará o Nome **da** página e o Modelo **de** página de uma página AEM para a Adobe Analytics.

### Objetivos {#objective}

1. Criar uma regra orientada por evento no Launch com base nas alterações feitas na camada de dados
1. Mapear propriedades de camada de dados de página para Elementos de dados no Launch
1. Coletar dados da página e enviar para a Adobe Analytics com o beacon de visualização da página

## Pré-requisitos

São necessários os seguintes:

* **Propriedade Experience Platform Launch**
* **ID do conjunto de relatórios de teste/desenvolvimento da Adobe Analytics** e servidor de rastreamento. Consulte a documentação a seguir para [criar um novo conjunto](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)de relatórios.
* [extensão do navegador do Depurador](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) de Experience Platform. Capturas de tela neste tutorial capturadas do navegador Chrome.
* (Opcional) AEM Site com a Camada de dados do cliente [Adobe. ativada](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial usará o site público [https://wknd.site/us/en.html](https://wknd.site/us/en.html) , mas você pode usar seu próprio site.

>[!NOTE]
>
> Precisa de ajuda para integrar o Launch ao seu site AEM? [Veja esta série](../experience-platform-launch/overview.md)de vídeos.

## Alternar Ambientes de inicialização para o site WKND

[https://wknd.site](https://wknd.site) é um site público criado com base em [um projeto](https://github.com/adobe/aem-guides-wknd) de código aberto projetado como referência e [tutorial](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) para implementações AEM.

Em vez de configurar um ambiente AEM e instalar a base de código WKND, você pode usar o depurador Experience Platform para **alternar** o live [https://wknd.site/](https://wknd.site/) para a *sua* propriedade Launch. É claro que você pode usar seu próprio site AEM se ele já tiver a Camada de Dados do Cliente [Adobe](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Faça logon no Experience Platform Launch e [crie uma propriedade](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html) Launch (caso ainda não tenha feito isso).
1. Certifique-se de que uma [Biblioteca de inicialização tenha sido criada](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library) e promovida para um [ambiente](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)de lançamento.
1. Copie o código incorporado do Launch do ambiente para o qual sua biblioteca foi publicada.

   ![Copiar código incorporado de inicialização](assets/collect-data-analytics/launch-environment-copy.png)

1. No navegador, abra uma nova guia e navegue até [https://wknd.site/](https://wknd.site/)
1. Abra a extensão do navegador do Experience Platform Debugger

   ![Depurador de Experience Platform](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navegue até **Iniciar** > **Configuração** e, em Códigos **incorporados** injetados, substitua o código incorporado Launch existente pelo *seu* código incorporado copiado da etapa 3.

   ![Substituir código incorporado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Ative o registro **do** console e **bloqueie** o depurador na guia WKND.

   ![Registro do console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verificar a camada de dados do cliente Adobe no site WKND

O projeto [de referência](https://github.com/adobe/aem-guides-wknd) WKND foi criado com AEM Componentes principais e tem a Camada de dados do cliente [Adobe ativada](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) por padrão. Em seguida, verifique se a Camada de dados do cliente Adobe está ativada.

1. Navegue até [https://wknd.site](https://wknd.site).
1. Abra as ferramentas do desenvolvedor do navegador e navegue até o **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   Isso retorna o estado atual da Camada de Dados do Cliente Adobe.

   ![Estado da camada de dados Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda a resposta e inspecione a `page` entrada. Você deve ver um schema de dados como o seguinte:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: "WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world."
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Usaremos as propriedades padrão derivadas do schema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)Página, `dc:title`e da camada de dados `xdm:language` `xdm:template` para enviar dados de página para a Adobe Analytics.

   >[!NOTE]
   >
   > Não vê o objeto `adobeDataLayer` javascript? Certifique-se de que a Camada de Dados do Cliente [Adobe esteja ativada](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) no site.

## Criar uma regra de página carregada

A Camada de Dados do Cliente Adobe é uma camada de dados orientada por **eventos** . Quando a camada de dados AEM **Página** é carregada, ela acionará um evento `cmp:show`. Crie uma regra que será acionada com base no `cmp:show` evento.

1. Navegue até Experience Platform Launch e na propriedade da Web integrada ao Site AEM.
1. Navegue até a seção **Regras** na interface de usuário Iniciar e clique em **Criar nova regra**.

   ![Criar regra](assets/collect-data-analytics/analytics-create-rule.png)

1. Nomeie a regra como **Página carregada**.
1. Clique em **Eventos** **Adicionar** para abrir o assistente de Configuração **de** Eventos.
1. Em **Tipo de evento** , selecione Código **** personalizado.

   ![Nomeie a regra e adicione o evento de código personalizado](assets/collect-data-analytics/custom-code-event.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   O trecho de código acima adicionará um ouvinte de evento [empurrando uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) para dentro da camada de dados. Quando o `cmp:show` evento é acionado, a `pageShownEventHandler` função é chamada. Nessa função, algumas verificações de integridade são adicionadas e uma nova `event` é construída com o [estado mais recente da camada](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) de dados do componente que acionou o evento.

   Depois isso `trigger(event)` é chamado. `trigger()` é um nome reservado no Launch e &quot;acionará&quot; a regra de inicialização. Enviamos o `event` objeto como um parâmetro que, por sua vez, será exposto por outro nome reservado no Launch chamado `event`. Os Elementos de dados no Launch agora podem fazer referência a várias propriedades, como: `event.component['someKey']`.

1. Salve as alterações.
1. Em **Ações** , clique em **Adicionar** para abrir o assistente de configuração **de** ação.
1. Em Tipo **de** ação, escolha Código **** personalizado.

   ![Tipo de ação de código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   O `event` objeto é transmitido pelo `trigger()` método chamado no evento personalizado. `component` é a página atual derivada da camada de dados `getState` no evento personalizado. Lembre-se do início do schema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) Página exposto pela camada de dados para ver as várias teclas expostas da caixa.

1. Salve as alterações e execute uma [compilação](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) no Launch para promover o código para o [ambiente](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) usado no seu Site AEM.

   >[!NOTE]
   >
   > Pode ser muito útil usar o [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) para alternar o código incorporado para um ambiente **de desenvolvimento** .

1. Navegue até o site AEM e abra as ferramentas do desenvolvedor para visualização do console. Atualize a página e você deverá ver que as mensagens do console foram registradas:

   ![Mensagens do console carregado da página](assets/collect-data-analytics/page-show-event-console.png)

## Criar elementos de dados

Em seguida, crie vários Elementos de dados para capturar valores diferentes da Camada de dados do cliente Adobe. Conforme observado no exercício anterior, vimos que é possível acessar as propriedades da camada de dados diretamente por meio do código personalizado. A vantagem de usar os Elementos de dados é que eles podem ser reutilizados nas regras de lançamento.

Lembre-se do início do schema [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) Página exposto pela camada de dados:

Os elementos de dados serão mapeados para as propriedades `@type`, `dc:title`e `xdm:template` .

### Tipo de recurso do componente

1. Navegue até Experience Platform Launch e na propriedade da Web integrada ao Site AEM.
1. Navegue até a seção Elementos **de** dados e clique em **Criar novo elemento** de dados.
1. Em **Nome** , digite Tipo **de Recurso** do Componente.
1. Para Tipo **de elemento de** dados, selecione Código **** personalizado.

   ![Tipo de recurso do componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   Salve as alterações.

   >[!NOTE]
   >
   > Lembre-se de que o `event` objeto está disponível e com escopo com base no evento que acionou a **Regra** na Inicialização. O valor de um Elemento de dados não é definido até que o Elemento de dados seja *referenciado* em uma Regra. Portanto, é seguro usar esse Elemento de dados dentro de uma Regra como a regra **Página carregada** criada na etapa anterior, *mas* não é seguro usar em outros contextos.

### Nome da Página

1. Clique em **Adicionar elemento** de dados.
1. Em **Nome** , digite Nome **** da página.
1. Para Tipo **de elemento de** dados, selecione Código **** personalizado.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salve as alterações.

### Modelo da página

1. Clique em **Adicionar elemento** de dados.
1. Em **Nome** , digite Nome **** da página.
1. Para Tipo **de elemento de** dados, selecione Código **** personalizado.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Salve as alterações.

1. Agora você deve ter três elementos de dados como parte da sua regra:

   ![Elementos de dados na regra](assets/collect-data-analytics/data-elements-page-rule.png)

## Adicionar a extensão do Analytics

Em seguida, adicione a extensão do Analytics à propriedade Launch. Precisamos enviar esses dados para algum lugar!

1. Navegue até Experience Platform Launch e na propriedade da Web integrada ao Site AEM.
1. Ir para **Extensões** > **Catálogo**
1. Localize a extensão **Adobe Analytics** e clique em **Instalar**

   ![Extensão Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Em Gerenciamento **de** biblioteca > Conjuntos **de** relatórios, digite as IDs do conjunto de relatórios que você gostaria de usar com cada ambiente do Launch.

   ![Insira as IDs do conjunto de relatórios](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Não há problema em usar um conjunto de relatórios para todos os ambientes neste tutorial, mas na vida real você gostaria de usar conjuntos de relatórios separados, como mostrado na imagem abaixo

   >[!TIP]
   >
   >Recomendamos usar a opção ** Gerenciar a biblioteca como a configuração Gerenciamento de biblioteca, pois facilita muito a atualização da `AppMeasurement.js` biblioteca.

1. Marque a caixa para ativar **Usar Activity Map**.

   ![Habilitar uso de Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. Em **Geral** > Servidor **de** rastreamento, digite seu servidor de rastreamento, por exemplo, `tmd.sc.omtrdc.net`. Digite seu Servidor de rastreamento SSL se o site suportar `https://`

   ![Insira os servidores de rastreamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Click **Save** to save the changes.

## Adicionar uma condição à regra Página carregada

Em seguida, atualize a regra **Página carregada** para usar o elemento de dados Tipo **de recurso de** componente para garantir que a regra só seja acionada quando o `cmp:show` evento for para a **Página**. Outros componentes podem acionar o `cmp:show` evento, por exemplo, o componente Carrossel o acionará quando os slides mudarem. Portanto, é importante adicionar uma condição para esta regra.

1. Na interface de usuário Iniciar, navegue até a regra **Página carregada** criada anteriormente.
1. Em **Condições** , clique em **Adicionar** para abrir o assistente de Configuração **de** condição.
1. Para Tipo **de** condição, selecione Comparação **de valor**.
1. Defina o primeiro valor no campo de formulário como `%Component Resource Type%`. Você pode usar o ícone ![de elemento de](assets/collect-data-analytics/cylinder-icon.png) dados Ícone de elemento de dados para selecionar o elemento de dados Tipo **de recurso de** componente. Deixe o comparador definido como `Equals`.
1. Defina o segundo valor como `wknd/components/page`.

   ![Configuração da condição para a regra carregada da página](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > É possível adicionar essa condição dentro da função de código personalizada que escuta o `cmp:show` evento criado anteriormente no tutorial. No entanto, a adição na interface do usuário dá mais visibilidade a usuários adicionais que podem precisar fazer alterações na regra. Além disso, usamos nosso elemento de dados!

1. Salve as alterações.

## Definir variáveis do Analytics e acionar o beacon de Visualização da página

Atualmente, a regra **Página carregada** simplesmente gera uma declaração de console. Em seguida, use os elementos de dados e a extensão do Analytics para definir as variáveis do Analytics como uma **ação** na regra **Página carregada** . Também definiremos uma ação adicional para acionar o beacon **de Visualização da** página e enviar os dados coletados para a Adobe Analytics.

1. Na regra **Página carregada** , **remova** a ação Código **** principal - personalizado (as instruções do console):

   ![Remover ação de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. Em Ações, clique em **Adicionar** para adicionar uma nova ação.
1. Defina o tipo de **extensão** como **Adobe Analytics** e defina o Tipo **de** ação como **Definir variáveis**

   ![Definir extensão de ação para definir variáveis do Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. No painel principal, selecione um **eVar** disponível e defina como o valor do Modelo **de** página do elemento de dados. Use o ícone Elementos de dados ícone Elementos ![de dados ícone](assets/collect-data-analytics/cylinder-icon.png) para selecionar o elemento Modelo **de** página.

   ![Definir como modelo de página de eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Role para baixo, em Configurações **** adicionais, defina Nome **da** página para o elemento de dados Nome **da** página:

   ![Conjunto da variável de Ambiente Nome da página](assets/collect-data-analytics/page-name-env-variable-set.png)

   Salve as alterações.

1. Em seguida, adicione uma Ação adicional à direita de **Adobe Analytics - Definir variáveis** tocando no ícone de **adição** :

   ![Adicionar uma ação de inicialização adicional](assets/collect-data-analytics/add-additional-launch-action.png)

1. Defina o tipo de **extensão** como **Adobe Analytics** e defina o Tipo **de** ação como **Enviar beacon**. Como isso é considerado uma visualização de página, deixe o rastreamento padrão definido como **`s.t()`**.

   ![Ação Enviar beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salve as alterações. A regra **Página carregada** agora deve ter a seguinte configuração:

   ![Configuração de inicialização final](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escute o `cmp:show` evento.
   * **2.** Verifique se o evento foi acionado por uma página.
   * **3.** Definir variáveis do Analytics para Nome **de** página e Modelo de **página**
   * **4.** Enviar o sinal de Visualização da página do Analytics
1. Salve todas as alterações e crie a biblioteca do Launch, promovendo o Ambiente apropriado.

## Validar a chamada Beacon de Visualização da página e do Analytics

Agora que a regra **Página carregada** envia o beacon do Analytics, você deve ser capaz de visualizar as variáveis de rastreamento do Analytics usando o Depurador de Experience Platform.

1. Abra o Site [](https://wknd.site/us/en.html) WKND no seu navegador.
1. Clique no ícone Depurador ícone ![Experience platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Verifique se o Depurador está mapeando a propriedade Launch para *seu* ambiente de desenvolvimento, conforme descrito anteriormente, e se **Console Logging** está marcado.
1. Abra o menu Análises e verifique se o conjunto de relatórios está definido como *seu* conjunto de relatórios. O Nome da página também deve ser preenchido:

   ![Depurador da guia Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Role para baixo e expanda Solicitações **de** rede. Você deve encontrar a **evar** definida para o Modelo **de** página:

   ![Evar e conjunto Nome da página](assets/collect-data-analytics/evar-page-name-set.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Clique no **carrossel** na parte superior da página.

   ![Clique na página do carrossel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe no console do navegador a declaração do console:

   ![Condição não atendida](assets/collect-data-analytics/condition-not-met.png)

   Isso ocorre porque o carrossel aciona um `cmp:show` evento, *mas* devido à verificação do Tipo **de recurso do** componente, nenhum evento é acionado.

   >[!NOTE]
   >
   > Se você não vir nenhum log do console, verifique se **Console Logging** está marcado em **Launch** (Inicialização) no Experience Platform Debugger.

1. Navegue até uma página de artigo como a Austrália [](https://wknd.site/us/en/magazine/western-australia.html)Ocidental. Observe que o Nome da página e o Tipo de modelo são alterados.

## Parabéns!

Você acabou de usar a Camada de Dados e o Experience Platform Launch para Cliente do Adobe orientado pelo evento para coletar dados da página de dados de um Site AEM e enviá-los para a Adobe Analytics.

### Próximas etapas

Consulte o tutorial a seguir para saber como usar a camada de Dados do cliente do Adobe orientada por evento para [rastrear cliques de componentes específicos em um site](track-clicked-component.md)da Adobe Experience Manager.
