---
title: Coletar dados de página com o Adobe Analytics
description: Use a camada Dados do cliente Adobe orientada por eventos para coletar dados sobre a atividade do usuário em um site criado com o Adobe Experience Manager. Saiba como usar as regras no Experience Platform Launch para acompanhar esses eventos e enviar dados para um conjunto de relatórios do Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: ef1fe712921bd5516cb389862cacf226a71aa193
workflow-type: tm+mt
source-wordcount: '2371'
ht-degree: 2%

---

# Coletar dados de página com o Adobe Analytics

Saiba como usar os recursos integrados do [Camada de dados do cliente do Adobe com componentes principais de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR) para coletar dados sobre uma página no Adobe Experience Manager Sites. [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) e [Extensão do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) são usadas para criar regras para enviar dados de página para o Adobe Analytics.

## O que você vai criar

![Rastreamento de dados da página](assets/collect-data-analytics/analytics-page-data-tracking.png)

Neste tutorial, você acionará uma regra do Launch com base em um evento da Camada de dados do cliente do Adobe, adicionará condições para quando a regra deve ser acionada e enviará a variável **Nome da página** e **Modelo de página** de uma Página AEM para o Adobe Analytics.

### Objetivos {#objective}

1. Criar uma regra orientada por eventos no Launch com base em alterações na camada de dados
1. Mapear as propriedades da camada de dados da página para Elementos de dados no Launch
1. Colete dados de página e envie para o Adobe Analytics com o beacon de exibição de página

## Pré-requisitos

Os seguintes itens são obrigatórios:

* **Experience Platform Launch** Propriedade
* **Adobe Analytics** ID do conjunto de relatórios de teste/desenvolvimento e servidor de rastreamento. Consulte a documentação a seguir para [criação de um novo conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) extensão do navegador. Capturas de tela neste tutorial capturadas pelo navegador Chrome.
* (Opcional) AEM o site com a variável [Camada de dados do cliente Adobe habilitado](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial usará o site voltado para o público [https://wknd.site/us/en.html](https://wknd.site/us/en.html) mas você é bem-vindo a usar seu próprio site.

>[!NOTE]
>
> Precisa de ajuda para integrar o Launch e seu site de AEM? [Ver esta série de vídeos](../experience-platform/data-collection/tags/overview.md).

## Alternar ambientes do Launch para o site WKND

[https://wknd.site](https://wknd.site) é um site aberto ao público criado com base em [um projeto de código aberto](https://github.com/adobe/aem-guides-wknd) concebido como referência e [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR) para implementações de AEM.

Em vez de configurar um ambiente de AEM e instalar a base de código WKND, você pode usar o Experience Platform Debugger para **switch** ao vivo [https://wknd.site/](https://wknd.site/) para *your* Propriedade do Launch. É claro que você pode usar seu próprio site AEM se ele já tiver o [Camada de dados do cliente Adobe habilitado](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Faça logon no Experience Platform Launch e [criar uma propriedade do Launch](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (caso ainda não o tenha feito).
1. Certifique-se de que um lançamento inicial [A biblioteca foi criada](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) e promovidas a um lançamento [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. Copie o código incorporado do Launch do ambiente no qual a Biblioteca foi publicada.

   ![Copiar código incorporado do Launch](assets/collect-data-analytics/launch-environment-copy.png)

1. No navegador, abra uma nova guia e navegue até [https://wknd.site/](https://wknd.site/)
1. Abra a extensão do navegador do Experience Platform Debugger

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navegar para **Launch** > **Configuração** e **Códigos Incorporados Inseridos** substitua o código incorporado existente do Launch por *your* código incorporado copiado da etapa 3.

   ![Substituir código incorporado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Habilitar **Logon do console** e **Bloquear** o depurador na guia WKND.

   ![Logon do console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verificar a camada de dados do cliente do Adobe no site WKND

O [Projeto de referência WKND](https://github.com/adobe/aem-guides-wknd) é criada com AEM componentes principais e tem o [Camada de dados do cliente Adobe habilitado](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) por padrão. Em seguida, verifique se a Camada de dados do cliente do Adobe está ativada.

1. Navegar para [https://wknd.site](https://wknd.site).
1. Abra as ferramentas do desenvolvedor do navegador e navegue até o **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   Isso retorna o estado atual da Camada de dados do cliente do Adobe.

   ![Estado da camada de dados Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda a resposta e inspecione o `page` entrada. Você deve ver um schema de dados como o seguinte:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Usaremos as propriedades padrão derivadas do [Schema da página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page),  `dc:title`, `xdm:language` e `xdm:template` da camada de dados para enviar dados da página para o Adobe Analytics.

   >[!NOTE]
   >
   > Não veja o `adobeDataLayer` objeto javascript? Certifique-se de que [A Camada de Dados do Cliente Adobe foi ativada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) no seu site.

## Criar uma regra de Página carregada

A camada de dados do cliente do Adobe é um **evento** camada de dados orientada. Quando o AEM **Página** a camada de dados é carregada, isso acionará um evento `cmp:show`. Crie uma regra que seja acionada com base no `cmp:show` evento.

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Navegue até o **Regras** na interface do usuário do Launch e, em seguida, clique em **Criar nova regra**.

   ![Criar regra](assets/collect-data-analytics/analytics-create-rule.png)

1. Atribua um nome à regra **Página carregada**.
1. Clique em **Eventos** **Adicionar** para abrir o **Configuração de evento** assistente.
1. Em **Tipo de evento** select **Código personalizado**.

   ![Nomeie a regra e adicione o evento de código personalizado](assets/collect-data-analytics/custom-code-event.png)

1. Clique em **Abrir editor** no painel principal e digite o seguinte trecho de código:

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

   O trecho de código acima adicionará um ouvinte de evento ao [forçar uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) na camada de dados. Quando a variável `cmp:show` for acionado `pageShownEventHandler` é chamada. Nesta função, são adicionadas algumas verificações de conformidade e uma nova `event` é construído com o mais recente [estado da camada de dados](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para o componente que acionou o evento.

   Depois disso `trigger(event)` é chamado. `trigger()` é um nome reservado no Launch e &quot;acionará&quot; a Regra do Launch. Passamos o `event` objeto como um parâmetro que, por sua vez, é exposto por outro nome reservado no Launch chamado `event`. Os elementos de dados no Launch agora podem fazer referência a várias propriedades da seguinte maneira: `event.component['someKey']`.

1. Salve as alterações.
1. Próximo em **Ações** click **Adicionar** para abrir o **Configuração de ação** assistente.
1. Em **Tipo de ação** Choose **Código personalizado**.

   ![Tipo de ação do código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e digite o seguinte trecho de código:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   O `event` é passado do `trigger()` chamado no evento personalizado. `component` é a página atual derivada da camada de dados `getState` no evento personalizado . Lembre-se de que [Schema da página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposto pela camada de dados para ver as várias chaves expostas prontas para uso.

1. Salve as alterações e execute um [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) no Launch para promover o código à variável [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) usado no seu site AEM.

   >[!NOTE]
   >
   > Pode ser muito útil usar a variável [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) para alternar o código incorporado para um **Desenvolvimento** ambiente.

1. Navegue até o seu site de AEM e abra as ferramentas do desenvolvedor para visualizar o console. Atualize a página e você verá que as mensagens do console foram registradas:

   ![Mensagens do console carregadas na página](assets/collect-data-analytics/page-show-event-console.png)

## Criar elementos de dados

Em seguida, crie vários Elementos de dados para capturar valores diferentes da Camada de dados do cliente do Adobe. Conforme observado no exercício anterior, vimos que é possível acessar as propriedades da camada de dados diretamente por meio do código personalizado. A vantagem de usar Elementos de dados é que eles podem ser reutilizados nas regras do Launch.

Lembre-se de que [Schema da página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposto pela camada de dados:

Os elementos de dados são mapeados para a variável `@type`, `dc:title`e `xdm:template` propriedades.

### Tipo de recurso do componente

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Navegue até o **Elementos de dados** e clique em **Criar novo elemento de dados**.
1. Para **Nome** enter **Tipo de recurso do componente**.
1. Para **Tipo de elemento de dados** select **Código personalizado**.

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
   > Lembre-se que a variável `event` O objeto é disponibilizado e tem escopo com base no evento que acionou a variável **Regra** no Launch. O valor de um elemento de dados não é definido até que o elemento de dados seja *referenciado* em uma regra. Portanto, é seguro usar esse Elemento de dados dentro de uma Regra como a **Página carregada** regra criada na etapa anterior *but* não seria seguro usar em outros contextos.

### Nome da Página

1. Clique em **Adicionar elemento de dados**.
1. Para **Nome** enter **Nome da página**.
1. Para **Tipo de elemento de dados** select **Código personalizado**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salve as alterações.

### Modelo da página

1. Clique em **Adicionar elemento de dados**.
1. Para **Nome** enter **Modelo de página**.
1. Para **Tipo de elemento de dados** select **Código personalizado**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   Salve as alterações.

1. Agora você deve ter três elementos de dados como parte de sua regra:

   ![Elementos de dados na regra](assets/collect-data-analytics/data-elements-page-rule.png)

## Adicionar a extensão Analytics

Em seguida, adicione a extensão do Analytics à propriedade do Launch. Precisamos enviar esses dados para algum lugar!

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Ir para **Extensões** > **Catálogo**
1. Localize a variável **Adobe Analytics** e clique em **Instalar**

   ![Extensão do Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Em **Gerenciamento de biblioteca** > **Conjuntos de relatórios**, insira as IDs do conjunto de relatórios que você deseja usar com cada ambiente do Launch.

   ![Insira as IDs do conjunto de relatórios](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Não há problema em usar um conjunto de relatórios para todos os ambientes neste tutorial, mas na vida real você pode usar conjuntos de relatórios separados, como mostrado na imagem abaixo

   >[!TIP]
   >
   >Recomendamos usar o *Opção Gerenciar a biblioteca para mim* como a configuração Gerenciamento de biblioteca , pois facilita muito a manutenção da variável `AppMeasurement.js` biblioteca atualizada.

1. Marque a caixa para ativar **Use o Activity Map**.

   ![Ativar o Activity Map de uso](assets/track-clicked-component/analytic-track-click.png)

1. Em **Geral** > **Servidor de rastreamento**, insira o servidor de rastreamento, por exemplo `tmd.sc.omtrdc.net`. Insira seu servidor de rastreamento SSL se o site suporta `https://`

   ![Insira os servidores de rastreamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Clique em **Salvar** para salvar as alterações.

## Adicionar uma condição à regra Página carregada

Em seguida, atualize o **Página carregada** regra para usar o **Tipo de recurso do componente** elemento de dados para garantir que a regra só seja acionada quando a variável `cmp:show` é para a variável **Página**. Outros componentes podem acionar o `cmp:show` , por exemplo, o componente Carrossel será acionado quando os slides forem alterados. Portanto, é importante adicionar uma condição para essa regra.

1. Na interface do usuário do Launch, navegue até o **Página carregada** regra criada anteriormente.
1. Em **Condições** click **Adicionar** para abrir o **Configuração de condição** assistente.
1. Para **Tipo de condição** select **Comparação de valores**.
1. Defina o primeiro valor no campo de formulário como `%Component Resource Type%`. Você pode usar o ícone Elemento de dados ![ícone do elemento de dados](assets/collect-data-analytics/cylinder-icon.png) para selecionar o **Tipo de recurso do componente** elemento de dados. Deixe o comparador definido como `Equals`.
1. Defina o segundo valor como `wknd/components/page`.

   ![Configuração de condição para a regra de página carregada](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > É possível adicionar essa condição dentro da função de código personalizado que escuta a variável `cmp:show` evento criado anteriormente no tutorial. No entanto, adicioná-la na interface do usuário dá mais visibilidade a usuários adicionais que podem precisar fazer alterações na regra. Além disso, usamos nosso elemento de dados!

1. Salve as alterações.

## Definir variáveis do Analytics e acionar o sinal de Exibição de página

Atualmente, o **Página carregada** A regra simplesmente gera uma instrução do console. Em seguida, use os elementos de dados e a extensão do Analytics para definir as variáveis do Analytics como uma **ação** no **Página carregada** regra. Também definiremos uma ação adicional para acionar a variável **Beacon de exibição de página** e enviar os dados coletados para a Adobe Analytics.

1. No **Página carregada** regra **remove** o **Core - Código personalizado** ação (as instruções do console):

   ![Remover ação de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. Em Ações, clique em **Adicionar** para adicionar uma nova ação.
1. Defina as **Extensão** digitar para **Adobe Analytics** e defina a **Tipo de ação** para  **Definir variáveis**

   ![Definir a extensão de ação para definir variáveis do Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. No painel principal, selecione um **eVar** e definido como o valor do Elemento de dados **Modelo de página**. Usar o ícone Elementos de dados ![Ícone Elementos de dados](assets/collect-data-analytics/cylinder-icon.png) para selecionar o **Modelo de página** elemento.

   ![Definir como modelo de página de eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Role para baixo, em baixo **Configurações adicionais** set **Nome da página** ao elemento de dados **Nome da página**:

   ![Nome da página Conjunto da variável de ambiente](assets/collect-data-analytics/page-name-env-variable-set.png)

   Salve as alterações.

1. Em seguida, adicione uma Ação adicional à direita do **Adobe Analytics - Definir variáveis** tocando no **plus** ícone :

   ![Adicionar uma ação de lançamento adicional](assets/collect-data-analytics/add-additional-launch-action.png)

1. Defina as **Extensão** digitar para **Adobe Analytics** e defina a **Tipo de ação** para  **Enviar beacon**. Como isso é considerado uma exibição de página, deixe o rastreamento padrão definido como **`s.t()`**.

   ![Ação Enviar beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salve as alterações. O **Página carregada** a regra deve ter a seguinte configuração:

   ![Configuração final de inicialização](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escute o `cmp:show` evento.
   * **2.** Verifique se o evento foi acionado por uma página.
   * **3.** Definir variáveis do Analytics para **Nome da página** e **Modelo de página**
   * **4.** Enviar o sinal de Exibição de página do Analytics
1. Salve todas as alterações e crie a biblioteca do Launch, promovendo para o ambiente apropriado.

## Validar o sinal de Exibição de página e a chamada do Analytics

Agora que a variável **Página carregada** envia o beacon do Analytics, você deve conseguir ver as variáveis de rastreamento do Analytics usando o Experience Platform Debugger.

1. Abra o [Site WKND](https://wknd.site/us/en.html) no seu navegador.
1. Clique no ícone do Debugger ![Ícone do Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Certifique-se de que o Debugger esteja mapeando a propriedade do Launch para *your* Ambiente de desenvolvimento, conforme descrito anteriormente e **Logon do console** está marcada.
1. Abra o menu do Analytics e verifique se o conjunto de relatórios está definido como *your* conjunto de relatórios. O Nome da página também deve ser preenchido:

   ![Depurador de guia do Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Role para baixo e expanda **Solicitações de rede**. Você deve ser capaz de encontrar a variável **evar** definido para o **Modelo de página**:

   ![Evar e Nome da Página definidos](assets/collect-data-analytics/evar-page-name-set.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Clique no botão **Carrossel** na parte superior da página.

   ![Clique na página do carrossel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe no console do navegador a declaração do console:

   ![Condição não atendida](assets/collect-data-analytics/condition-not-met.png)

   Isso ocorre porque o carrossel aciona uma `cmp:show` evento *but* devido à verificação da **Tipo de recurso do componente**, nenhum evento é acionado.

   >[!NOTE]
   >
   > Se você não vir nenhum log do console, verifique se **Logon do console** está marcado em **Launch** no Experience Platform Debugger.

1. Navegue até uma página de artigo como [Austrália Ocidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que Nome da página e Tipo de modelo são alterados.

## Parabéns!

Você acabou de usar a Adobe Client Data Layer e a Experience Platform Launch para coletar dados de página de dados de um Site de AEM e enviá-los para a Adobe Analytics.

### Próximas etapas

Consulte o tutorial a seguir para saber como usar a camada de dados do cliente do Adobe orientada por eventos para [rastrear cliques de componentes específicos em um site do Adobe Experience Manager](track-clicked-component.md).
