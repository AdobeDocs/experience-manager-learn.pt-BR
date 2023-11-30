---
title: Integração do AEM Sites ao Adobe Analytics com a extensão de tags da Adobe Analytics
description: Integre o AEM Sites com o Adobe Analytics, usando a camada Dados do cliente Adobe orientada por eventos para coletar dados sobre a atividade do usuário em um site criado com o Adobe Experience Manager. Saiba como usar as regras de tag para acompanhar esses eventos e enviar dados para um conjunto de relatórios do Adobe Analytics.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="Integração" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 3%

---

# Integrar o AEM Sites e o Adobe Analytics

>[!NOTE]
>
>O Adobe Experience Platform Launch foi reformulado como um conjunto de tecnologias de coleção de dados na Adobe Experience Platform. Como resultado, várias alterações de terminologia foram implementadas na documentação do produto. Consulte o seguinte [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) para obter uma referência consolidada das alterações de terminologia.


Saiba como integrar o AEM Sites e o Adobe Analytics à extensão de tags da Adobe Analytics usando os recursos integrados do [Camada de dados de clientes Adobe com componentes principais AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR) para coletar dados sobre uma página no Adobe Experience Manager Sites. [Tags no Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) e a variável [Extensão do Adobe Analytics](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) são usadas para criar regras para enviar dados de página para o Adobe Analytics.

## O que você vai criar {#what-build}

![Rastreamento de dados da página](assets/collect-data-analytics/analytics-page-data-tracking.png)

Neste tutorial, você acionará uma regra de tag com base em um evento da Camada de dados do cliente Adobe. Além disso, adicione condições para quando a regra deve ser acionada e envie o **Nome da página** e **Modelo da página** valores de uma página AEM para o Adobe Analytics.

### Objetivos {#objective}

1. Crie uma regra orientada por eventos na propriedade de tag que capture alterações da camada de dados
1. Mapear propriedades da camada de dados da página para Elementos de dados na propriedade da tag
1. Coletar e enviar dados de página para o Adobe Analytics usando o sinal de exibição de página

## Pré-requisitos

Os seguintes são obrigatórios:

* **Propriedade da tag** no Experience Platform
* **Adobe Analytics** ID do conjunto de relatórios de teste/desenvolvimento e servidor de rastreamento. Consulte a documentação a seguir para [criação de um conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) extensão do navegador. Capturas de tela neste tutorial capturadas do navegador Chrome.
* (Opcional) Site AEM com a variável [Camada de dados de clientes Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial usa informações voltadas ao público [WKND](https://wknd.site/us/en.html) site, mas você pode usar seu próprio site.

>[!NOTE]
>
> Precisa de ajuda para integrar a propriedade de tag e o site do AEM? [Ver esta série de vídeos](../experience-platform/data-collection/tags/overview.md).

## Alternar ambiente de tag para o site da WKND

A variável [WKND](https://wknd.site/us/en.html) é um site voltado para o público, criado com base em [um projeto de código aberto](https://github.com/adobe/aem-guides-wknd) concebido como referência e [tutorial](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=pt-BR) para uma implementação do AEM.

Em vez de configurar um ambiente AEM e instalar a base de código WKND, você pode usar o depurador Experience Platform para **alternar** o live [Site da WKND](https://wknd.site/us/en.html) para *seu* propriedade da tag. No entanto, você pode usar seu próprio site AEM se ele já tiver o [Camada de dados de clientes Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Faça logon no Experience Platform e [criar uma propriedade de tag](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (caso ainda não o tenha feito).
1. Verifique se uma tag inicial do JavaScript [a biblioteca foi criada](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) e promovido para a tag [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. Copie o código de incorporação do JavaScript do ambiente de tag em que sua biblioteca foi publicada.

   ![Copiar código de inserção da propriedade de tag](assets/collect-data-analytics/launch-environment-copy.png)

1. No navegador, abra uma nova guia e navegue até [Site da WKND](https://wknd.site/us/en.html)
1. Abra a extensão do navegador Experience Platform Debugger

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navegue até **Tags do Experience Platform** > **Configuração** e sob **Códigos de inserção inseridos** substituir o código incorporado existente por *seu* código incorporado copiado da etapa 3.

   ![Substituir código de inserção](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Ativar **Registro do console** e **Bloquear** o depurador na guia WKND.

   ![Registro do console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verificar a camada de dados do cliente Adobe no site WKND

A variável [Projeto de referência WKND](https://github.com/adobe/aem-guides-wknd) O é construído com os Componentes principais do AEM e tem o [Camada de dados de clientes Adobe habilitada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) por padrão. Em seguida, verifique se a Camada de dados do cliente Adobe está ativada.

1. Navegue até [Site da WKND](https://wknd.site/us/en.html).
1. Abra as ferramentas de desenvolvedor do navegador e acesse o **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   O código acima retorna o estado atual da Camada de dados de clientes Adobe.

   ![Estado da Camada de Dados Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda a resposta e inspecione as `page` entrada. Você deve ver um schema de dados como o seguinte:

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

   Para enviar dados de página para o Adobe Analytics, vamos usar as propriedades padrão, como `dc:title`, `xdm:language`, e `xdm:template` da camada de dados.

   Para obter mais informações, consulte [Esquema de página](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) dos Esquemas de dados dos Componentes principais.

   >[!NOTE]
   >
   > Se você não vir a variável `adobeDataLayer` Objeto JavaScript? Certifique-se de que o [A Camada de Dados de Clientes Adobe foi ativada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) no seu site.

## Criar uma regra de Página carregada

A Camada de dados de clientes Adobe é uma **orientado por eventos** camada de dados. Quando a camada de dados da página AEM é carregada, ela aciona um `cmp:show` evento. Crie uma regra que seja acionada quando o `cmp:show` é acionado a partir da camada de dados da página.

1. Navegue até o Experience Platform e para a propriedade de tag integrada ao site AEM.
1. Navegue até a **Regras** na interface de usuário da Propriedade de tag e clique em **Criar nova regra**.

   ![Criar regra](assets/collect-data-analytics/analytics-create-rule.png)

1. Atribua um nome à regra **Página carregada**.
1. No **Eventos** , clique em **Adicionar** para abrir o **Configuração de evento** assistente.
1. Para **Tipo de evento** selecione **Custom Code**.

   ![Nomeie a regra e adicione o evento de código personalizado](assets/collect-data-analytics/custom-code-event.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
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

   O trecho de código acima adiciona um ouvinte de evento de [envio de uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) na camada de dados. Quando `cmp:show` evento é acionado o `pageShownEventHandler` é chamada. Nesta função, algumas verificações de sanidade são adicionadas e um novo `event` é construído com o mais recente [estado da camada de dados](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para o componente que acionou o evento.

   Por último, a `trigger(event)` é chamada. A variável `trigger()` é um nome reservado na propriedade de tag e ele **acionadores** a regra. A variável `event` object é transmitido como um parâmetro que, por sua vez, é exposto por outro nome reservado na propriedade tag. Os elementos de dados na propriedade da tag agora podem fazer referência a várias propriedades usando um trecho de código como `event.component['someKey']`.

1. Salve as alterações.
1. Próximo em **Ações** click **Adicionar** para abrir o **Configuração de ação** assistente.
1. Para **Tipo de ação** escolha **Custom Code**.

   ![Tipo de ação do código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   A variável `event` o objeto é transmitido de `trigger()` chamado no evento personalizado. Aqui, a variável `component` é a página atual derivada da camada de dados `getState` no evento personalizado.

1. Salve as alterações e execute um [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) na propriedade da tag para promover o código à [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) usado no seu site AEM.

   >[!NOTE]
   >
   > Pode ser útil usar a variável [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) para alternar o código incorporado para um **Desenvolvimento** ambiente.

1. Navegue até o site AEM e abra as ferramentas do desenvolvedor para visualizar o console. Atualize a página e você verá que as mensagens do console foram registradas:

![Mensagens de console carregadas na página](assets/collect-data-analytics/page-show-event-console.png)

## Criar elementos de dados

Em seguida, crie vários Elementos de dados para capturar valores diferentes da Camada de dados do cliente Adobe. Como visto no exercício anterior, é possível acessar as propriedades da camada de dados diretamente pelo código personalizado. A vantagem de usar elementos de dados é que eles podem ser reutilizados nas regras de tag.

Os elementos de dados são mapeados para o `@type`, `dc:title`, e `xdm:template` propriedades.

### Tipo de recurso do componente

1. Navegue até o Experience Platform e para a propriedade de tag integrada ao site AEM.
1. Navegue até a **Elementos de dados** e clique em **Criar novo elemento de dados**.
1. Para o **Nome** insira o **Tipo de recurso do componente**.
1. Para o **Tipo de elemento de dados** selecione **Custom Code**.

   ![Tipo de recurso do componente](assets/collect-data-analytics/component-resource-type-form.png)

1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. Salve as alterações.

   >[!NOTE]
   >
   > Lembre-se de que a `event` objeto é disponibilizado e escopo com base no evento que acionou o **Regra** na propriedade da tag. O valor de um elemento de dados não é definido até que o elemento de dados seja *referenciado* em uma Regra. Portanto, é seguro usar esse elemento de dados em uma Regra como a **Página carregada** regra criada na etapa anterior *mas* não seria seguro usar em outros contextos.

### Nome da Página

1. Clique em **Adicionar elemento de dados** botão
1. Para o **Nome** insira **Nome da página**.
1. Para o **Tipo de elemento de dados** selecione **Custom Code**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. Salve as alterações.

### Modelo da página

1. Clique em **Adicionar elemento de dados** botão
1. Para o **Nome** insira **Modelo da página**.
1. Para o **Tipo de elemento de dados** selecione **Custom Code**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. Salve as alterações.

1. Agora você deve ter três elementos de dados como parte da regra:

   ![Elementos de dados na regra](assets/collect-data-analytics/data-elements-page-rule.png)

## Adicionar a extensão Analytics

Em seguida, adicione a extensão do Analytics à propriedade de tag para enviar dados a um conjunto de relatórios.

1. Navegue até o Experience Platform e para a propriedade de tag integrada ao site AEM.
1. Ir para **Extensões** > **Catálogo**
1. Localize o **Adobe Analytics** e clique em **Instalar**

   ![Extensão do Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Em **Gerenciamento de biblioteca** > **Conjuntos de relatórios**, insira as IDs do conjunto de relatórios que você deseja usar com cada ambiente de tag.

   ![Insira as IDs do conjunto de relatórios](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Não há problema em usar um conjunto de relatórios para todos os ambientes neste tutorial, mas na vida real você pode usar conjuntos de relatórios separados, como mostrado na imagem abaixo

   >[!TIP]
   >
   >Recomendamos usar o *Opção Gerenciar a biblioteca para mim* como a configuração Gerenciamento de biblioteca, pois facilita muito a manutenção da variável `AppMeasurement.js` biblioteca atualizada.

1. Marque a caixa para ativar **Usar Activity Map**.

   ![Ativar Activity Map de uso](assets/track-clicked-component/analytic-track-click.png)

1. Em **Geral** > **Servidor de rastreamento**, insira o servidor de rastreamento, por exemplo, `tmd.sc.omtrdc.net`. Insira seu servidor de rastreamento SSL se o site suporta `https://`

   ![Insira os servidores de rastreamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Clique em **Salvar** para salvar as alterações.

## Adicionar uma condição à regra Página carregada

Em seguida, atualize o **Página carregada** regra para usar o **Tipo de recurso do componente** elemento de dados para garantir que a regra só seja acionada quando o `cmp:show` evento é para o **Página**. Outros componentes podem acionar o `cmp:show` por exemplo, o componente Carrossel é acionado quando os slides são alterados. Portanto, é importante adicionar uma condição para essa regra.

1. Na interface da Propriedade de tag, navegue até a **Página carregada** regra criada anteriormente.
1. Em **Condições** click **Adicionar** para abrir o **Configuração de condição** assistente.
1. Para **Tipo de condição** selecione **Value Comparison** opção.
1. Defina o primeiro valor no campo de formulário como `%Component Resource Type%`. Você pode usar o ícone Elemento de dados ![ícone de elemento de dados](assets/collect-data-analytics/cylinder-icon.png) para selecionar o **Tipo de recurso do componente** elemento de dados. Deixe o comparador definido como `Equals`.
1. Defina o segundo valor como `wknd/components/page`.

   ![Configuração de condição para a regra de página carregada](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > É possível adicionar essa condição na função de código personalizada que atende à `cmp:show` evento criado anteriormente no tutorial. No entanto, adicioná-la na interface do dá mais visibilidade a usuários adicionais que podem precisar fazer alterações na regra. Além disso, podemos usar nosso elemento de dados!

1. Salve as alterações.

## Definir variáveis do Analytics e acionar o sinal de Exibição de página

Atualmente, o **Página carregada** A regra do simplesmente gera uma instrução do console. Em seguida, use os elementos de dados e a extensão Analytics para definir as variáveis do Analytics como uma **ação** no **Página carregada** regra. Também definimos uma ação extra para acionar o **Beacon de Exibição de página** e enviar os dados coletados para o Adobe Analytics.

1. Na regra Página carregada, **remover** o **Núcleo - Código personalizado** ação (as instruções do console):

   ![Remover ação de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. Na subseção Ações, clique em **Adicionar** para adicionar uma nova ação.

1. Defina o **Extensão** digite para **Adobe Analytics** e defina o **Tipo de ação** para  **Definir variáveis**

   ![Definir extensão de ação para variáveis de conjunto do Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. No painel principal, selecione uma opção disponível **eVar** e definido como o valor do Elemento de dados **Modelo da página**. Usar o ícone Elementos de dados ![Ícone de elementos de dados](assets/collect-data-analytics/cylinder-icon.png) para selecionar o **Modelo da página** elemento.

   ![Definir como modelo da página de eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Role para baixo, em **Configurações adicionais** set **Nome da página** ao elemento de dados **Nome da página**:

   ![Conjunto de variáveis de ambiente do nome da página](assets/collect-data-analytics/page-name-env-variable-set.png)

1. Salve as alterações.

1. Em seguida, adicione uma Ação extra à direita do **Adobe Analytics - Definir variáveis** tocando no **mais** ícone:

   ![Adicionar uma ação de regra de tag adicional](assets/collect-data-analytics/add-additional-launch-action.png)

1. Defina o **Extensão** digite para **Adobe Analytics** e defina o **Tipo de ação** para  **Enviar sinal**. Como essa ação é considerada uma exibição de página, deixe o rastreamento padrão definido como **`s.t()`**.

   ![Enviar ação Beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salve as alterações. A variável **Página carregada** A regra do agora deve ter a seguinte configuração:

   ![Configuração da regra de tag final](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Ouça o `cmp:show` evento.
   * **2.** Verifique se o evento foi acionado por uma página.
   * **3.** Definir variáveis do Analytics para **Nome da página** e **Modelo da página**
   * **4.** Enviar o sinal de Exibição de página do Analytics

1. Salve todas as alterações e crie sua biblioteca de tags, promovendo para o ambiente apropriado.

## Validar o sinal de Exibição de página e a chamada do Analytics

Agora que o **Página carregada** Se uma regra enviar o beacon do Analytics, você poderá ver as variáveis de rastreamento do Analytics usando o Depurador de Experience Platform.

1. Abra o [Site da WKND](https://wknd.site/us/en.html) no navegador.
1. Clique no ícone Depurador ![Ícone do Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Verifique se o Depurador está mapeando a propriedade da tag para *seu* Ambiente de desenvolvimento, conforme descrito anteriormente e **Registro do console** está marcado.
1. Abra o menu Analytics e verifique se o conjunto de relatórios está definido como *seu* conjunto de relatórios. O Nome da página também deve ser preenchido:

   ![Depurador de guia do Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Rolar para baixo e expandir **Solicitações de rede**. Você deverá conseguir encontrar o **evar** definido para o **Modelo da página**:

   ![Evar e Nome da página definidos](assets/collect-data-analytics/evar-page-name-set.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Clique na guia **Carrossel** na parte superior da página.

   ![Clique na página do carrossel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe a instrução do console no console do navegador:

   ![Condição não atendida](assets/collect-data-analytics/condition-not-met.png)

   Isso ocorre porque o Carrossel aciona um `cmp:show` evento *mas* devido à nossa verificação do **Tipo de recurso do componente**, nenhum evento é acionado.

   >[!NOTE]
   >
   > Se você não visualizar nenhum log do console, verifique se **Registro do console** está marcado em **Tags do Experience Platform** no Experience Platform Debugger.

1. Acesse uma página de artigo como [Austrália Ocidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que o Nome da página e o Tipo de modelo são alterados.

## Parabéns.

Você acabou de usar a Camada de dados do cliente Adobe orientada por eventos e as Tags no Experience Platform para coletar dados da página de dados de um site AEM e enviá-los para o Adobe Analytics.

### Próximas etapas

Dê uma olhada no tutorial a seguir para saber como usar a camada de dados do cliente Adobe orientada por eventos para [rastrear cliques de componentes específicos em um site do Adobe Experience Manager](track-clicked-component.md).
