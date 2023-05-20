---
title: Rastrear componente clicado com o Adobe Analytics
description: Use a camada Dados do cliente Adobe orientada por eventos para rastrear cliques de componentes específicos em um site do Adobe Experience Manager. Saiba como usar as regras de tag para acompanhar esses eventos e enviar dados para um conjunto de relatórios do Adobe Analytics usando um beacon de rastreamento de link.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 1%

---

# Rastrear componente clicado com o Adobe Analytics

>[!NOTE]
>
>O Adobe Experience Platform Launch foi reformulado como um conjunto de tecnologias de coleção de dados na Adobe Experience Platform. Como resultado, várias alterações de terminologia foram implementadas na documentação do produto. Consulte o seguinte [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para obter uma referência consolidada das alterações de terminologia.

Usar o orientado por eventos [Camada de dados de clientes Adobe com componentes principais AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR) para rastrear cliques de componentes específicos em um site do Adobe Experience Manager. Saiba como usar regras na propriedade de tag para acompanhar eventos de clique, filtrar por componente e enviar os dados para uma Adobe Analytics com um beacon de rastreamento de link.

## O que você vai criar {#what-build}

A equipe de marketing da WKND está interessada em saber quais `Call to Action (CTA)` Os botões do têm melhor desempenho na página inicial. Neste tutorial, vamos adicionar uma regra à propriedade de tag que acompanha o `cmp:click` eventos de **Teaser** e **Botão** componentes. Em seguida, envie a ID do componente e um novo evento para a Adobe Analytics junto com o beacon de rastreamento de link.

![O que você criará para rastrear cliques](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Crie uma regra orientada por eventos na propriedade de tag que capture a variável `cmp:click` evento.
1. Filtre os diferentes eventos por tipo de recurso do componente.
1. Defina a ID do componente e envie um evento para a Adobe Analytics com o sinal de rastreamento de link.

## Pré-requisitos

Este tutorial é uma continuação do [Coletar dados de página com o Adobe Analytics](./collect-data-analytics.md) e o pressupõe que você tenha:

* A **Propriedade da tag** com o [Extensão do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) habilitado
* **Adobe Analytics** ID do conjunto de relatórios de teste/desenvolvimento e servidor de rastreamento. Consulte a documentação a seguir para [criação de um conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Depurador Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) extensão do navegador configurada com a propriedade da tag carregada no [Site da WKND](https://wknd.site/us/en.html) ou um site AEM com a Camada de dados de Adobe ativada.

## Inspect do esquema de botão e teaser

Antes de criar regras na propriedade da tag, é útil revisar a [esquema do botão e do teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspecioná-los na implementação da camada de dados.

1. Navegue até [Página inicial da WKND](https://wknd.site/us/en.html)
1. Abra as ferramentas de desenvolvedor do navegador e acesse o **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   O código acima retorna o estado atual da Camada de dados de clientes Adobe.

   ![Estado da camada de dados por meio do console do navegador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Expandir a resposta e localizar entradas com o prefixo `button-` e  `teaser-xyz-cta` entrada. Você deve ver um schema de dados como o seguinte:

   Esquema do botão:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Esquema de teaser:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Os dados acima baseiam-se no [Esquema de itens de Componentes/Contêineres](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). A nova regra de tag usa esse esquema.

## Criar uma regra clicada do CTA

A Camada de dados de clientes Adobe é uma **evento** camada de dados orientada. Sempre que qualquer Componente principal for clicado em um `cmp:click` O evento é despachado por meio da camada de dados. Para ouvir o `cmp:click` vamos criar uma regra .

1. Navegue até o Experience Platform e para a propriedade de tag integrada ao site AEM.
1. Navegue até a **Regras** na interface de usuário da Propriedade de tag e clique em **Adicionar regra**.
1. Atribua um nome à regra **CTA clicado**.
1. Clique em **Eventos** > **Adicionar** para abrir o **Configuração de evento** assistente.
1. Para **Tipo de evento** selecione **Custom Code**.

   ![Nomeie a regra de CTA clicado e adicione o evento de código personalizado](assets/track-clicked-component/custom-code-event.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

   O trecho de código acima adiciona um ouvinte de evento de [envio de uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) na camada de dados. Sempre que a variável `cmp:click` evento é acionado o `componentClickedHandler` é chamada. Nesta função, algumas verificações de sanidade são adicionadas e um novo `event` objeto é construído com o mais recente [estado da camada de dados](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para o componente que acionou o evento.

   Por último, a `trigger(event)` é chamada. A variável `trigger()` é um nome reservado na propriedade de tag e ele **acionadores** a regra. A variável `event` object é transmitido como um parâmetro que, por sua vez, é exposto por outro nome reservado na propriedade tag. Os elementos de dados na propriedade da tag agora podem fazer referência a várias propriedades usando um trecho de código como `event.component['someKey']`.

1. Salve as alterações.
1. Próximo em **Ações** click **Adicionar** para abrir o **Configuração de ação** assistente.
1. Para **Tipo de ação** escolha **Custom Code**.

   ![Tipo de ação do código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   A variável `event` o objeto é transmitido de `trigger()` chamado no evento personalizado. A variável `component` objeto é o estado atual do componente derivado da camada de dados `getState()` e é o elemento que provocou o clique.

1. Salve as alterações e execute um [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) na propriedade da tag para promover o código à [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) usado no seu site AEM.

   >[!NOTE]
   >
   > Pode ser útil usar a variável [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) para alternar o código incorporado para um **Desenvolvimento** ambiente.

1. Navegue até a [Site da WKND](https://wknd.site/us/en.html) e abra as ferramentas do desenvolvedor para visualizar o console. Além disso, selecione a variável **Preservar log** caixa de seleção

1. Clique em uma das opções **Teaser** ou **Botão** Botões CTA para navegar para outra página.

   ![Botão CTA para clicar](assets/track-clicked-component/cta-button-to-click.png)

1. Observe no console do desenvolvedor que a variável **CTA clicado** a regra foi acionada:

   ![Botão CTA clicado](assets/track-clicked-component/cta-button-clicked-log.png)

## Criar elementos de dados

Em seguida, crie um Elemento de dados para capturar a ID do componente e o título que foi clicado. Recordar, no exercício anterior, os resultados do `event.path` era algo semelhante a `component.button-b6562c963d` e o valor de `event.component['dc:title']` era algo como &quot;Ver viagens&quot;.

### ID do componente

1. Navegue até o Experience Platform e para a propriedade de tag integrada ao site AEM.
1. Navegue até a **Elementos de dados** e clique em **Adicionar novo elemento de dados**.
1. Para **Nome** insira **ID do componente**.
1. Para **Tipo de elemento de dados** selecione **Custom Code**.

   ![Formulário Elemento de dados ID do componente](assets/track-clicked-component/component-id-data-element.png)

1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. Salve as alterações.

   >[!NOTE]
   >
   > Lembre-se de que a `event` objeto é disponibilizado e escopo com base no evento que acionou o **Regra** na propriedade da tag. O valor de um elemento de dados não é definido até que o elemento de dados seja *referenciado* em uma Regra. Portanto, é seguro usar esse elemento de dados em uma Regra como a **Página carregada** regra criada na etapa anterior *mas* não seria seguro usar em outros contextos.


### Título do componente

1. Navegue até a **Elementos de dados** e clique em **Adicionar novo elemento de dados**.
1. Para **Nome** insira **Título do componente**.
1. Para **Tipo de elemento de dados** selecione **Custom Code**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Salve as alterações.

## Adicionar uma condição à regra CTA Clicado

Em seguida, atualize o **CTA clicado** regra para garantir que a regra só seja acionada quando a variável `cmp:click` evento é acionado por um **Teaser** ou um **Botão**. Como o CTA do Teaser é considerado um objeto separado na camada de dados, é importante verificar se o pai veio de um Teaser.

1. Na interface da Propriedade de tag, navegue até a **CTA clicado** regra criada anteriormente.
1. Em **Condições** click **Adicionar** para abrir o **Configuração de condição** assistente.
1. Para **Tipo de condição** selecione **Custom Code**.

   ![Código personalizado de condição clicada do CTA](assets/track-clicked-component/custom-code-condition.png)

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

   O código acima verifica primeiro se o tipo de recurso era de um **Botão** ou se o tipo de recurso foi de um CTA em um **Teaser**.

1. Salve as alterações.

## Definir variáveis do Analytics e acionar o beacon de rastreamento de link

Atualmente, o **CTA clicado** A regra do simplesmente gera uma instrução do console. Em seguida, use os elementos de dados e a extensão Analytics para definir as variáveis do Analytics como uma **ação**. Vamos também definir uma ação extra para acionar o **Rastrear link** e enviar os dados coletados para o Adobe Analytics.

1. No **CTA clicado** regra, **remover** o **Núcleo - Código personalizado** ação (as instruções do console):

   ![Remover ação de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. Em Ações, clique em **Adicionar** para criar uma ação.
1. Defina o **Extensão** digite para **Adobe Analytics** e defina o **Tipo de ação** para  **Definir variáveis**.

1. Defina os seguintes valores para **eVars**, **Props**, e **Eventos**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Definir Propriedade de eVar e eventos](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aqui `%Component ID%` é usado, pois garante um identificador exclusivo para o CTA que foi clicado. Uma possível desvantagem de usar `%Component ID%` o relatório do Analytics contém valores como `button-2e6d32893a`. Usar o `%Component Title%` daria um nome mais amigável, mas o valor pode não ser exclusivo.

1. Em seguida, adicione uma Ação extra à direita do **Adobe Analytics - Definir variáveis** tocando no **mais** ícone:

   ![Adicionar uma ação extra à regra de tag](assets/track-clicked-component/add-additional-launch-action.png)

1. Defina o **Extensão** digite para **Adobe Analytics** e defina o **Tipo de ação** para  **Enviar sinal**.
1. Em **Rastreamento** defina o botão de opção como **`s.tl()`**.
1. Para **Tipo de link** escolha **Link Personalizado** e para **Nome do link** defina o valor como: **`%Component Title%: CTA Clicked`**:

   ![Configuração do beacon de Enviar link](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   A configuração acima combina a variável dinâmica do elemento de dados **Título do componente** e a string estática **CTA clicado**.

1. Salve as alterações. A variável **CTA clicado** A regra do agora deve ter a seguinte configuração:

   ![Configuração da regra de tag final](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Ouça o `cmp:click` evento.
   * **2.** Verifique se o evento foi acionado por um **Botão** ou **Teaser**.
   * **3.** Definir variáveis do Analytics para rastrear o **ID do componente** como um **eVar**, **prop**, e uma **evento**.
   * **4.** Envie o beacon de rastreamento de link do Analytics (e **não** trate-o como uma exibição de página).

1. Salve todas as alterações e crie sua biblioteca de tags, promovendo para o ambiente apropriado.

## Validar o beacon de rastreamento de link e a chamada do Analytics

Agora que o **CTA clicado** Se uma regra enviar o beacon do Analytics, você poderá ver as variáveis de rastreamento do Analytics usando o Depurador de Experience Platform.

1. Abra o [Site da WKND](https://wknd.site/us/en.html) no navegador.
1. Clique no ícone Depurador ![Ícone do Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Verifique se o Depurador está mapeando a propriedade da tag para *seu* ambiente de desenvolvimento, conforme descrito anteriormente, e a **Registro do console** está marcado.
1. Abra o menu Analytics e verifique se o conjunto de relatórios está definido como *seu* conjunto de relatórios.

   ![Depurador de guia do Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. No navegador, clique em uma das opções **Teaser** ou **Botão** Botões CTA para navegar para outra página.

   ![Botão CTA para clicar](assets/track-clicked-component/cta-button-to-click.png)

1. Retorne ao Depurador de Experience Platform e role para baixo e expanda **Solicitações de rede** > *Seu conjunto de relatórios*. Você deverá conseguir encontrar o **eVar**, **prop**, e **evento** definido.

   ![Eventos, evar e prop do Analytics rastreados no clique](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Navegue até o rodapé do site e clique em um dos links de navegação:

   ![Clique no link Navegação no rodapé](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe a mensagem no console do navegador *O &quot;Código personalizado&quot; para a regra &quot;CTA clicado&quot; não foi atendido*.

   A mensagem acima ocorre porque o componente de Navegação aciona um `cmp:click` evento *mas* devido a [Condição para a regra](#add-a-condition-to-the-cta-clicked-rule) que verifica o tipo de recurso em que nenhuma ação é tomada.

   >[!NOTE]
   >
   > Se você não visualizar nenhum log do console, verifique se **Registro do console** está marcado em **Tags do Experience Platform** no Experience Platform Debugger.

## Parabéns!

Você acabou de usar a Camada de dados do cliente Adobe orientada por eventos e a Tag no Experience Platform para rastrear os cliques de componentes específicos em um site de AEM.
