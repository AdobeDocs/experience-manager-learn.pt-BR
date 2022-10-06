---
title: Rastrear componente clicado com o Adobe Analytics
description: Use a camada Dados do cliente do Adobe orientada por eventos para rastrear cliques de componentes específicos em um site do Adobe Experience Manager. Saiba como usar as regras no Experience Platform Launch para acompanhar esses eventos e enviar dados para uma Adobe Analytics com um beacon de rastreamento de link.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 3%

---

# Rastrear componente clicado com o Adobe Analytics

Usar a função orientada por evento [Camada de dados do cliente do Adobe com componentes principais de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR) para rastrear cliques de componentes específicos em um site do Adobe Experience Manager. Saiba como usar as regras no Experience Platform Launch para acompanhar eventos de clique, filtrar por componente e enviar os dados ao Adobe Analytics com um aviso de rastreamento de link.

## O que você vai criar

A equipe de marketing da WKND quer entender quais botões de Chamada para Ação (CTA) estão tendo o melhor desempenho na página inicial. Neste tutorial, adicionaremos uma nova regra no Experience Platform Launch que escuta `cmp:click` eventos de **Teaser** e **Botão** e enviar a ID do componente e um novo evento para o Adobe Analytics ao lado do sinal de rastreamento de link.

![O que você criará para rastrear cliques](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### Objetivos {#objective}

1. Crie uma regra orientada por eventos no Launch com base na variável `cmp:click` evento.
1. Filtre os diferentes eventos por tipo de recurso de componente.
1. Defina a ID do componente clicada e envie o Adobe Analytics do evento com o beacon de rastreamento de link.

## Pré-requisitos

Este tutorial é uma continuação do [Coletar dados de página com o Adobe Analytics](./collect-data-analytics.md) e parte do princípio que você tem:

* A **Propriedade do Launch** com o [Extensão do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) ativado
* **Adobe Analytics** ID do conjunto de relatórios de teste/desenvolvimento e servidor de rastreamento. Consulte a documentação a seguir para [criação de um novo conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) extensão do navegador configurada com a propriedade do Launch carregada em [https://wknd.site/us/en.html](https://wknd.site/us/en.html) ou um site AEM com a Camada de dados Adobe.

## Inspect o esquema do botão e do teaser

Antes de criar regras no Launch, é útil revisar a variável [esquema para o Botão e Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) e inspecione-as na implementação da camada de dados.

1. Navegar para [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Abra as ferramentas do desenvolvedor do navegador e navegue até o **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   Isso retorna o estado atual da Camada de dados do cliente do Adobe.

   ![Estado da camada de dados por meio do console do navegador](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. Expanda as entradas de resposta e localização com o prefixo `button-` e  `teaser-xyz-cta` entrada. Você deve ver um schema de dados como o seguinte:

   Esquema do botão:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Esquema do Teaser:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   Eles se baseiam na variável [Esquema de Item do Componente/Contêiner](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). A regra que criaremos no Launch usará esse schema.

## Criar uma regra CTA clicada

A camada de dados do cliente do Adobe é um **evento** camada de dados orientada. Quando qualquer Componente principal é clicado em um `cmp:click` é despachado por meio da camada de dados. Em seguida, crie uma regra para escutar a `cmp:click` evento.

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Navegue até o **Regras** na interface do usuário do Launch e, em seguida, clique em **Adicionar regra**.
1. Atribua um nome à regra **CTA clicado**.
1. Clique em **Eventos** > **Adicionar** para abrir o **Configuração de evento** assistente.
1. Em **Tipo de evento** select **Código personalizado**.

   ![Nomeie a regra CTA clicado e adicione o evento de código personalizado](assets/track-clicked-component/custom-code-event.png)

1. Clique em **Abrir editor** no painel principal e digite o seguinte trecho de código:

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

   O trecho de código acima adiciona um ouvinte de evento por [forçar uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) na camada de dados. Quando a variável `cmp:click` for acionado `componentClickedHandler` é chamada. Nesta função, são adicionadas algumas verificações de conformidade e uma nova `event` é construído com o mais recente [estado da camada de dados](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para o componente que acionou o evento.

   Depois disso `trigger(event)` é chamado. `trigger()` é um nome reservado no Launch e &quot;aciona&quot; a Regra do Launch. Passamos o `event` objeto como um parâmetro que, por sua vez, é exposto por outro nome reservado no Launch chamado `event`. Os elementos de dados no Launch agora podem fazer referência a várias propriedades da seguinte maneira: `event.component['someKey']`.

1. Salve as alterações.
1. Próximo em **Ações** click **Adicionar** para abrir o **Configuração de ação** assistente.
1. Em **Tipo de ação** Choose **Código personalizado**.

   ![Tipo de ação do código personalizado](assets/track-clicked-component/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e digite o seguinte trecho de código:

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   O `event` é passado do `trigger()` chamado no evento personalizado. `component` é o estado atual do componente derivado da camada de dados `getState` que acionou o clique.

1. Salve as alterações e execute um [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) no Launch para promover o código à variável [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) usado no seu site AEM.

   >[!NOTE]
   >
   > Pode ser muito útil usar a variável [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) para alternar o código incorporado para um **Desenvolvimento** ambiente.

1. Navegue até o [Site WKND](https://wknd.site/us/en.html) e abra as ferramentas do desenvolvedor para visualizar o console. Selecionar **Preservar log**.

1. Clique em uma das **Teaser** ou **Botão** Botões CTA para navegar para outra página.

   ![Botão CTA para clicar](assets/track-clicked-component/cta-button-to-click.png)

1. Observe no console do desenvolvedor que a variável **CTA clicado** regra foi disparada:

   ![Botão CTA clicado](assets/track-clicked-component/cta-button-clicked-log.png)

## Criar elementos de dados

Em seguida, crie um Elemento de dados para capturar a ID do componente e o título clicados. Recordar no exercício anterior a produção de `event.path` era algo semelhante a `component.button-b6562c963d` e o valor de `event.component['dc:title']` era algo como &quot;Viagens de visualização&quot;.

### ID do componente

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Navegue até o **Elementos de dados** e clique em **Adicionar novo elemento de dados**.
1. Para **Nome** enter **ID do componente**.
1. Para **Tipo de elemento de dados** select **Código personalizado**.

   ![Formulário de elemento de dados da ID do componente](assets/track-clicked-component/component-id-data-element.png)

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
   > Lembre-se que a variável `event` O objeto é disponibilizado e tem escopo com base no evento que acionou a variável **Regra** no Launch. O valor de um elemento de dados não é definido até que o elemento de dados seja *referenciado* em uma regra. Portanto, é seguro usar esse Elemento de dados dentro de uma Regra como a **CTA clicado** regra criada no exercício anterior *but* não seria seguro usar em outros contextos.

### Título do componente

1. Navegue até o **Elementos de dados** e clique em **Adicionar novo elemento de dados**.
1. Para **Nome** enter **Título do componente**.
1. Para **Tipo de elemento de dados** select **Código personalizado**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salve as alterações.

## Adicionar uma condição à regra CTA clicado

Em seguida, atualize o **CTA clicado** para garantir que a regra só seja acionada quando a variável `cmp:click` é acionado para um **Teaser** ou **Botão**. Como o CTA do Teaser é considerado um objeto separado na camada de dados, é importante verificar se o pai veio de um Teaser.

1. Na interface do usuário do Launch, navegue até o **CTA clicado** regra criada anteriormente.
1. Em **Condições** click **Adicionar** para abrir o **Configuração de condição** assistente.
1. Para **Tipo de condição** select **Código personalizado**.

   ![Código personalizado de condição de CTA clicada](assets/track-clicked-component/custom-code-condition.png)

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

Atualmente, o **CTA clicado** A regra simplesmente gera uma instrução do console. Em seguida, use os elementos de dados e a extensão do Analytics para definir as variáveis do Analytics como uma **ação**. Também definiremos uma ação adicional para acionar a variável **Rastrear link** e enviar os dados coletados para a Adobe Analytics.

1. No **CTA clicado** regra **remove** o **Core - Código personalizado** ação (as instruções do console):

   ![Remover ação de código personalizado](assets/track-clicked-component/remove-console-statements.png)

1. Em Ações, clique em **Adicionar** para adicionar uma nova ação.
1. Defina as **Extensão** digitar para **Adobe Analytics** e defina a **Tipo de ação** para  **Definir variáveis**.

1. Defina os seguintes valores para **eVars**, **Props** e **Eventos**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![Definir Prop de eVar e eventos](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > Aqui `%Component ID%` é usada, pois garantirá um identificador exclusivo para o CTA que foi clicado. Uma possível desvantagem do uso `%Component ID%` é que o relatório do Analytics conterá valores como `button-2e6d32893a`. Usando `%Component Title%` daria um nome mais amigável, mas o valor pode não ser único.

1. Em seguida, adicione uma Ação adicional à direita do **Adobe Analytics - Definir variáveis** tocando no **plus** ícone :

   ![Adicionar uma ação de lançamento adicional](assets/track-clicked-component/add-additional-launch-action.png)

1. Defina as **Extensão** digitar para **Adobe Analytics** e defina a **Tipo de ação** para  **Enviar beacon**.
1. Em **Rastreamento** defina o botão de opção como **`s.tl()`**.
1. Para **Tipo de link** Choose **Link personalizado** e **Nome do link** defina o valor como: **`%Component Title%: CTA Clicked`**:

   ![Configuração do beacon Enviar link](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   Isso combinará a variável dinâmica do elemento de dados **Título do componente** e a string estática **CTA clicado**.

1. Salve as alterações. O **CTA clicado** a regra deve ter a seguinte configuração:

   ![Configuração final de inicialização](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** Escute o `cmp:click` evento.
   * **2.** Verifique se o evento foi acionado por um **Botão** ou **Teaser**.
   * **3.** Defina as variáveis do Analytics para rastrear o **ID do componente** como um **eVar**, **prop** e um **evento**.
   * **4.** Envie o sinal de rastreamento de link do Analytics (e faça **not** tratá-la como uma exibição de página).

1. Salve todas as alterações e crie a biblioteca do Launch, promovendo para o ambiente apropriado.

## Validar o sinal de rastreamento de link e a chamada do Analytics

Agora que a variável **CTA clicado** envia o beacon do Analytics, você deve conseguir ver as variáveis de rastreamento do Analytics usando o Experience Platform Debugger.

1. Abra o [Site WKND](https://wknd.site/us/en.html) no seu navegador.
1. Clique no ícone do Debugger ![Ícone do Experience Platform Debugger](assets/track-clicked-component/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Certifique-se de que o Debugger esteja mapeando a propriedade do Launch para *your* Ambiente de desenvolvimento, conforme descrito anteriormente e **Logon do console** está marcada.
1. Abra o menu do Analytics e verifique se o conjunto de relatórios está definido como *your* conjunto de relatórios.

   ![Depurador de guia do Analytics](assets/track-clicked-component/analytics-tab-debugger.png)

1. No navegador, clique em uma das **Teaser** ou **Botão** Botões CTA para navegar para outra página.

   ![Botão CTA para clicar](assets/track-clicked-component/cta-button-to-click.png)

1. Retorne ao Experience Platform Debugger e role para baixo e expanda **Solicitações de rede** > *Seu conjunto de relatórios*. Você deve ser capaz de encontrar a variável **eVar**, **prop** e **evento** definido.

   ![Eventos do Analytics, evar e prop rastreados ao clicar](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Navegue até o rodapé do site e clique em um dos links de navegação:

   ![Clique no link Navegação no rodapé](assets/track-clicked-component/click-navigation-link-footer.png)

1. Observe a mensagem no console do navegador *O &quot;Código personalizado&quot; para a regra &quot;CTA clicado&quot; não foi atendido*.

   Isso ocorre porque o componente Navegação aciona uma `cmp:click` evento *but* devido à verificação do recurso em relação ao tipo de recurso, nenhuma ação é executada.

   >[!NOTE]
   >
   > Se você não vir nenhum log do console, verifique se **Logon do console** está marcado em **Launch** no Experience Platform Debugger.

## Parabéns. 

Você acabou de usar a Adobe Client Data Layer e o Experience Platform Launch para rastrear cliques de componentes específicos em um site do Adobe Experience Manager.
