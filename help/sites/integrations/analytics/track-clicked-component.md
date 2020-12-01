---
title: Rastrear componente clicado com o Adobe Analytics
description: Use a camada de Dados do cliente Adobe orientada por evento para rastrear cliques de componentes específicos em um site da Adobe Experience Manager. Saiba como usar regras no Experience Platform Launch para acompanhar esses eventos e enviar dados para um Adobe Analytics com um beacon de rastreamento de link.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 64c167ec1d625fdd8be1bc56f7f5e59460b8fed3
workflow-type: tm+mt
source-wordcount: '1831'
ht-degree: 1%

---


# Rastrear componente clicado com o Adobe Analytics

Use a camada de dados do cliente [Adobe com componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/data-layer/overview.html) orientada pelo evento para rastrear cliques de componentes específicos em um site da Adobe Experience Manager. Saiba como usar regras no Experience Platform Launch para ouvir eventos de clique, filtrar por componente e enviar os dados para um Adobe Analytics com um beacon de rastreamento de link.

## O que você vai criar

A equipe de marketing da WKND quer entender quais botões Chamada para Ação (CTA) estão desempenhando o melhor no home page. Neste tutorial, adicionaremos uma nova regra no Experience Platform Launch que escuta eventos `cmp:click` dos componentes **Teaser** e **Botão** e enviaremos a ID do componente e um novo evento para a Adobe Analytics ao lado do beacon de rastreamento de link.

![O que você criará cliques de rastreamento](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Crie uma regra orientada por evento no Launch com base no evento `cmp:click`.
1. Filtre os diferentes eventos por tipo de recurso de componente.
1. Defina a ID do componente clicada e envie o evento Adobe Analytics com o beacon de rastreamento de link.

## Pré-requisitos

Este tutorial é uma continuação de [Coletar dados de página com o Adobe Analytics](./collect-data-analytics.md) e presume que você tenha:

* Uma **Iniciar propriedade** com a [extensão Adobe Analytics](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) ativada
* **ID do conjunto de relatórios do Adobe** Analytics/dev e servidor de rastreamento. Consulte a documentação a seguir para [criar um novo conjunto de relatórios](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Extensão do navegador ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Depurador de Experience Platform configurada com sua propriedade Launch carregada em  [https://wknd.site/us/en.](https://wknd.site/us/en.html) htmlor um site AEM com a Camada de dados Adobe.

## Inspect o Schema do botão e do teaser

Antes de fazer regras no Launch, é útil revisar o schema [para o Botão e Teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspecioná-los na implementação da camada de dados.

1. Navegue até [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra as ferramentas do desenvolvedor do navegador e navegue até **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   Isso retorna o estado atual da Camada de Dados do Cliente Adobe.

   ![Estado da camada de dados por meio do console do navegador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Expanda a resposta e localize as entradas com prefixo `button-` e `teaser-xyz-cta`. Você deve ver um schema de dados como o seguinte:

   Schema do botão:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Schema do Teaser:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Eles são baseados no Schema [Componente/Item de Container](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item). A regra que criaremos no Launch usará esse schema.

## Criar uma regra CTA clicada

A Camada de Dados do Cliente Adobe é uma camada de dados orientada por **evento**. Quando qualquer componente principal é clicado, um evento `cmp:click` será despachado pela camada de dados. Em seguida, crie uma regra para acompanhar o evento `cmp:click`.

1. Navegue até Experience Platform Launch e na propriedade da Web integrada ao Site AEM.
1. Navegue até a seção **Regras** na interface de usuário Iniciar e clique em **Adicionar regra**.
1. Nomeie a regra **CTA Clicada**.
1. Clique em **Eventos** > **Adicionar** para abrir o assistente **Configuração de Eventos**.
1. Em **Tipo de evento** selecione **Código Personalizado**.

   ![Nomeie a regra CTA Clicada e adicione o evento de código personalizado](assets/track-clicked-component/custom-code-event.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   O trecho de código acima adicionará um ouvinte de evento [empurrando uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) para a camada de dados. Quando o evento `cmp:click` é acionado, a função `componentClickedHandler` é chamada. Nesta função, algumas verificações de integridade são adicionadas e um novo objeto `event` é construído com o estado mais recente [da camada de dados](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para o componente que acionou o evento.

   Depois que `trigger(event)` for chamado. `trigger()` é um nome reservado no Launch e &quot;acionará&quot; a regra de inicialização. Enviamos o objeto `event` como um parâmetro que, por sua vez, será exposto por outro nome reservado no Launch chamado `event`. Os Elementos de dados no Launch agora podem fazer referência a várias propriedades, como: `event.component['someKey']`.

1. Salve as alterações.
1. Em **Ações**, clique em **Adicionar** para abrir o assistente **Configuração de Ação**.
1. Em **Tipo de ação** escolha **Código personalizado**.

   ![Tipo de ação de código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   O objeto `event` é transmitido pelo método `trigger()` chamado no evento personalizado. `component` é o estado atual do componente derivado da camada de dados  `getState` que acionou o clique.

1. Salve as alterações e execute um [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) no Launch para promover o código ao [ambiente](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) usado no seu Site AEM.

   >[!NOTE]
   >
   > Pode ser muito útil usar o [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) para alternar o código incorporado para um ambiente **Development**.

1. Navegue até [Site WKND](https://wknd.site/us/en.html) e abra as ferramentas do desenvolvedor para visualização do console. Selecione **Preservar log**.

1. Clique em um dos botões **Teaser** ou **Botão** CTA para navegar para outra página.

   ![Botão CTA para clicar](assets/track-clicked-component/cta-button-to-click.png)

1. Observe no console do desenvolvedor que a regra **CTA Clicked** foi acionada:

   ![Botão CTA clicado](assets/track-clicked-component/cta-button-clicked-log.png)

## Criar elementos de dados

Em seguida, crie um elemento de dados para capturar a ID do componente e o título clicados. Lembre-se que no exercício anterior a saída de `event.path` era semelhante a `component.button-b6562c963d` e o valor de `event.component['dc:title']` era algo como &quot;Visualizações de viagem&quot;.

### ID do componente

1. Navegue até Experience Platform Launch e na propriedade da Web integrada ao Site AEM.
1. Navegue até a seção **Elementos de dados** e clique em **Adicionar novo elemento de dados**.
1. Para **Nome** digite **ID do componente**.
1. Para **Tipo de elemento de dados** selecione **Código personalizado**.

   ![Formulário Elemento de dados da ID do componente](assets/track-clicked-component/component-id-data-element.png)

1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   Salve as alterações.

   >[!NOTE]
   >
   > Lembre-se de que o objeto `event` está disponível e com escopo com base no evento que acionou **Rule** no Launch. O valor de um Elemento de dados não é definido até que o Elemento de dados seja *referenciado* em uma Regra. Portanto, é seguro usar esse Elemento de dados dentro de uma Regra como a regra **CTA Clicked** criada no exercício anterior *mas* não seria segura para usar em outros contextos.

### Título do componente

1. Navegue até a seção **Elementos de dados** e clique em **Adicionar novo elemento de dados**.
1. Para **Nome** digite **Título do componente**.
1. Para **Tipo de elemento de dados** selecione **Código personalizado**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salve as alterações.

## Adicionar uma condição à regra CTA clicada

Em seguida, atualize a regra **CTA Clicada** para garantir que a regra só seja acionada quando o evento `cmp:click` for acionado para um **Teaser** ou um **Botão**. Como o CTA do Teaser é considerado um objeto separado na camada de dados, é importante verificar se o pai veio de um Teaser.

1. Na interface do usuário de inicialização, navegue até a regra **CTA Clicked** criada anteriormente.
1. Em **Condições**, clique em **Adicionar** para abrir o assistente **Configuração da Condição**.
1. Para **Tipo de condição** selecione **Código personalizado**.

   ![Código personalizado de condição clicada CTA](assets/track-clicked-component/custom-code-condition.png)

1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   O código acima verifica primeiro se o tipo de recurso era de um **Botão** e, em seguida, verifica se o tipo de recurso era de um CTA em um **Teaser**.

1. Salve as alterações.

## Definir variáveis do Analytics e acionar o beacon de rastreamento de link

Atualmente, a regra **CTA Clicked** simplesmente gera uma instrução de console. Em seguida, use os elementos de dados e a extensão do Analytics para definir as variáveis do Analytics como **action**. Também definiremos uma ação adicional para acionar o **Link de rastreamento** e enviar os dados coletados para a Adobe Analytics.

1. Na regra **CTA Clicked** **remove** a ação **Core - Custom Code** (as instruções do console):

   ![Remover ação de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. Em Ações, clique em **Adicionar** para adicionar uma nova ação.
1. Defina o tipo **Extension** como **Adobe Analytics** e defina o **Tipo de ação** como **Definir variáveis**.

1. Defina os seguintes valores para **eVars**, **Props** e **Eventos**:

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![Definir eVar e eventos](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aqui `%Component ID%` é usado, pois garantirá um identificador exclusivo para o CTA que foi clicado. Uma desvantagem potencial em usar `%Component ID%` é que o relatório do Analytics conterá valores como `button-2e6d32893a`. O uso de `%Component Title%` daria um nome mais amigável, mas o valor pode não ser exclusivo.

1. Em seguida, adicione uma Ação adicional à direita de **Adobe Analytics - Set Variables** tocando no ícone **mais**:

   ![Adicionar uma ação de inicialização adicional](assets/track-clicked-component/add-additional-launch-action.png)

1. Defina o tipo **Extension** como **Adobe Analytics** e defina o **Tipo de ação** como **Enviar beacon**.
1. Em **Tracking** defina o botão de opção como **`s.tl()`**.
1. Para **Tipo de link** escolha **Link personalizado** e para **Nome do link** defina o valor como: **`%Component Title%: CTA Clicked`**:

   ![Configuração para beacon Enviar link](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Isso combinará a variável dinâmica do elemento de dados **Título do componente** e a string estática **CTA Clicada**.

1. Salve as alterações. A regra **CTA Clicked** agora deve ter a seguinte configuração:

   ![Configuração de inicialização final](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Escute o  `cmp:click` evento.
   * **2.** Verifique se o evento foi acionado por um  **** Botão ou  **Teaser**.
   * **3.** Defina as variáveis do Analytics para rastrear a  **ID de** componente como um  **eVar**,  **prop** e um  **evento**.
   * **4.** Envie o beacon de link de rastreamento do Analytics (e  **** não o trate como uma visualização de página).

1. Salve todas as alterações e crie a biblioteca do Launch, promovendo o Ambiente apropriado.

## Validar a chamada Track Link Beacon e Analytics

Agora que a regra **CTA Clicked** envia o beacon do Analytics, você deve ser capaz de ver as variáveis de rastreamento do Analytics usando o Depurador de Experience Platform.

1. Abra o [Site WKND](https://wknd.site/us/en.html) no seu navegador.
1. Clique no ícone do Depurador ![ícone do Experience platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Verifique se o Depurador está mapeando a propriedade Launch para *seu* ambiente de desenvolvimento, conforme descrito anteriormente, e **Registro do console** está marcado.
1. Abra o menu Análises e verifique se o conjunto de relatórios está definido como *seu* conjunto de relatórios.

   ![Depurador da guia Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. No navegador, clique em um dos botões **Teaser** ou **Botão** CTA para navegar para outra página.

   ![Botão CTA para clicar](assets/track-clicked-component/cta-button-to-click.png)

1. Retorne ao Depurador de Experience Platform e role para baixo e expanda **Solicitações de rede** > *Seu conjunto de relatórios*. Você deve encontrar o conjunto **eVar**, **prop** e **evento**.

   ![Eventos, evar e prop do Analytics rastreados ao clicar](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Navegue até o rodapé do site e clique em um dos links de navegação:

   ![Clique no link Navegação no rodapé](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe no console do navegador que a mensagem *&quot;Código personalizado&quot; para a regra &quot;CTA clicado&quot; não foi atendida*.

   Isso ocorre porque o componente de Navegação aciona um evento `cmp:click` *mas* devido à verificação do objeto em relação ao tipo de recurso, nenhuma ação é executada.

   >[!NOTE]
   >
   > Se você não vir nenhum log do console, verifique se **Registro do console** está marcado em **Iniciar** no Depurador de Experience Platform.

## Parabéns!

Você acabou de usar a Camada de dados e o Experience Platform Launch para clientes orientados por Adobe para rastrear cliques de componentes específicos em um site da Adobe Experience Manager.