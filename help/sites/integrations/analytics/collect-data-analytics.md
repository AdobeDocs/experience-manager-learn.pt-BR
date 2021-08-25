---
title: Coletar dados de página com o Adobe Analytics
description: Use a camada Dados do cliente Adobe orientada por eventos para coletar dados sobre a atividade do usuário em um site criado com o Adobe Experience Manager. Saiba como usar as regras no Experience Platform Launch para acompanhar esses eventos e enviar dados para um conjunto de relatórios do Adobe Analytics.
version: cloud-service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '2375'
ht-degree: 1%

---


# Coletar dados de página com o Adobe Analytics

Saiba como usar os recursos integrados da Camada de dados do cliente do [Adobe com AEM Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) para coletar dados sobre uma página no Adobe Experience Manager Sites. [O Experience Platform ](https://www.adobe.com/experience-platform/launch.html) Launch e a extensão  [Adobe Analytics ](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) serão usados para criar regras para enviar dados de página ao Adobe Analytics.

## O que você vai criar

![Rastreamento de dados da página](assets/collect-data-analytics/analytics-page-data-tracking.png)

Neste tutorial, você acionará uma regra do Launch com base em um evento da Camada de dados do cliente do Adobe, adicionará condições para quando a regra deve ser acionada e enviará o **Nome da página** e **Modelo de página** de uma Página AEM para o Adobe Analytics.

### Objetivos {#objective}

1. Criar uma regra orientada por eventos no Launch com base em alterações na camada de dados
1. Mapear as propriedades da camada de dados da página para Elementos de dados no Launch
1. Colete dados de página e envie para o Adobe Analytics com o beacon de exibição de página

## Pré-requisitos

Os seguintes itens são obrigatórios:

* **Experience Platform** LaunchProperty
* **ID do conjunto de relatórios** do Adobe Analytics/dev e do servidor de rastreamento. Consulte a documentação a seguir para [criar um novo conjunto de relatórios](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Extensão ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) do navegador Experience Platform Debugger. Capturas de tela neste tutorial capturadas pelo navegador Chrome.
* (Opcional) AEM Site com a [Camada de dados do cliente do Adobe ativada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). Este tutorial usará o site voltado para o público [https://wknd.site/us/en.html](https://wknd.site/us/en.html), mas você é bem-vindo a usar seu próprio site.

>[!NOTE]
>
> Precisa de ajuda para integrar o Launch e seu site de AEM? [Veja esta série](../experience-platform-launch/overview.md) de vídeos.

## Alternar ambientes do Launch para o site WKND

[https://wknd.](https://wknd.site) O site é um site aberto público criado com base em  [um ](https://github.com/adobe/aem-guides-wknd) projeto de código aberto projetado como uma referência e  [](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) tutorial para implementações de AEM.

Em vez de configurar um ambiente de AEM e instalar a base de código WKND, você pode usar o Experience Platform Debugger para **alternar** o live [https://wknd.site/](https://wknd.site/) para *sua* Propriedade do Launch. É claro que você pode usar seu próprio site de AEM se ele já tiver a [Camada de dados do cliente do Adobe ativada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Faça logon no Experience Platform Launch e [crie uma propriedade do Launch](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (caso ainda não tenha feito isso).
1. Certifique-se de que uma biblioteca inicial do Launch [tenha sido criada](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) e promovida para um [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) do Launch.
1. Copie o código incorporado do Launch do ambiente no qual a Biblioteca foi publicada.

   ![Copiar código incorporado do Launch](assets/collect-data-analytics/launch-environment-copy.png)

1. No navegador, abra uma nova guia e navegue até [https://wknd.site/](https://wknd.site/)
1. Abra a extensão do navegador do Experience Platform Debugger

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. Navegue até **Launch** > **Configuração** e, em **Códigos incorporados inseridos**, substitua o código incorporado existente do Launch por *seu* código incorporado copiado da etapa 3.

   ![Substituir código incorporado](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. Ative **Registro do Console** e **Bloquear** o depurador na guia WKND.

   ![Logon do console](assets/collect-data-analytics/console-logging-lock-debugger.png)

## Verificar a camada de dados do cliente do Adobe no site WKND

O [projeto de referência WKND](https://github.com/adobe/aem-guides-wknd) é criado com AEM Componentes principais e tem a [Camada de dados do cliente Adobe ativada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) por padrão. Em seguida, verifique se a Camada de dados do cliente do Adobe está ativada.

1. Navegue até [https://wknd.site](https://wknd.site).
1. Abra as ferramentas do desenvolvedor do navegador e navegue até **Console**. Execute o seguinte comando:

   ```js
   adobeDataLayer.getState();
   ```

   Isso retorna o estado atual da Camada de dados do cliente do Adobe.

   ![Estado da camada de dados Adobe](assets/collect-data-analytics/adobe-data-layer-state.png)

1. Expanda a resposta e inspecione a entrada `page`. Você deve ver um schema de dados como o seguinte:

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

   Usaremos as propriedades padrão derivadas do [Page schema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` e `xdm:template` da camada de dados para enviar dados de página para o Adobe Analytics.

   >[!NOTE]
   >
   > Não vê o objeto javascript `adobeDataLayer`? Certifique-se de que a [Camada de Dados do Cliente do Adobe tenha sido ativada](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) no site.

## Criar uma regra de Página carregada

A Camada de dados do cliente do Adobe é uma camada de dados orientada por **evento**. Quando a camada de dados AEM **Page** é carregada, ela acionará um evento `cmp:show`. Crie uma regra que será acionada com base no evento `cmp:show` .

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Navegue até a seção **Regras** na interface do usuário do Launch e clique em **Criar nova regra**.

   ![Criar regra](assets/collect-data-analytics/analytics-create-rule.png)

1. Nomeie a regra **Página carregada**.
1. Clique em **Eventos** **Adicionar** para abrir o assistente de **Configuração de Evento**.
1. Em **Tipo de evento** selecione **Código personalizado**.

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

   O trecho de código acima adicionará um ouvinte de evento ao [enviar uma função](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) para a camada de dados. Quando o evento `cmp:show` é acionado, a função `pageShownEventHandler` é chamada. Nesta função, algumas verificações de integridade são adicionadas e um novo `event` é construído com o estado mais recente [da camada de dados](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) para o componente que acionou o evento.

   Depois que `trigger(event)` for chamado. `trigger()` é um nome reservado no Launch e &quot;acionará&quot; a Regra do Launch. Passamos o objeto `event` como um parâmetro que, por sua vez, será exposto por outro nome reservado no Launch chamado `event`. Os elementos de dados no Launch agora podem fazer referência a várias propriedades da seguinte maneira: `event.component['someKey']`.

1. Salve as alterações.
1. Em seguida, em **Actions** clique em **Add** para abrir o assistente **Action Configuration**.
1. Em **Tipo de ação** escolha **Código personalizado**.

   ![Tipo de ação do código personalizado](assets/collect-data-analytics/action-custom-code.png)

1. Clique em **Abrir editor** no painel principal e insira o seguinte trecho de código:

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   O objeto `event` é transmitido do método `trigger()` chamado no evento personalizado. `component` é a página atual derivada da camada de dados  `getState` no evento personalizado. Lembre-se de anteriormente do [Page schema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposto pela camada de dados para ver as várias chaves expostas imediatamente.

1. Salve as alterações e execute uma [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) no Launch para promover o código para o [ambiente](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) usado em seu Site de AEM.

   >[!NOTE]
   >
   > Pode ser muito útil usar o [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) para alternar o código incorporado para um ambiente **de desenvolvimento**.

1. Navegue até o seu site de AEM e abra as ferramentas do desenvolvedor para visualizar o console. Atualize a página e você verá que as mensagens do console foram registradas:

   ![Mensagens do console carregadas na página](assets/collect-data-analytics/page-show-event-console.png)

## Criar elementos de dados

Em seguida, crie vários Elementos de dados para capturar valores diferentes da Camada de dados do cliente do Adobe. Conforme observado no exercício anterior, vimos que é possível acessar as propriedades da camada de dados diretamente por meio do código personalizado. A vantagem de usar Elementos de dados é que eles podem ser reutilizados nas regras do Launch.

Lembre-se de antes do [Page schema](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) exposto pela camada de dados:

Os elementos de dados serão mapeados para as propriedades `@type`, `dc:title` e `xdm:template`.

### Tipo de recurso do componente

1. Navegue até o Experience Platform Launch e até a propriedade da Web integrada ao Site de AEM.
1. Navegue até a seção **Elementos de Dados** e clique em **Criar Novo Elemento de Dados**.
1. Para **Nome** digite **Tipo de Recurso de Componente**.
1. Para **Tipo de elemento de dados** selecione **Código personalizado**.

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
   > Lembre-se de que o objeto `event` é disponibilizado e com escopo com base no evento que acionou a **Regra** no Launch. O valor de um Elemento de dados não é definido até que o Elemento de dados seja *referenciado* em uma Regra. Portanto, é seguro usar esse Elemento de dados dentro de uma Regra como a regra **Página carregada** criada na etapa anterior *mas* não seria seguro usar em outros contextos.

### Nome da Página

1. Clique em **Adicionar elemento de dados**.
1. Para **Nome** digite **Nome da página**.
1. Para **Tipo de elemento de dados** selecione **Código personalizado**.
1. Clique em **Abrir editor** e insira o seguinte no editor de código personalizado:

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   Salve as alterações.

### Modelo da página

1. Clique em **Adicionar elemento de dados**.
1. Para **Nome** digite **Modelo de página**.
1. Para **Tipo de elemento de dados** selecione **Código personalizado**.
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
1. Vá para **Extensões** > **Catálogo**
1. Localize a extensão **Adobe Analytics** e clique em **Instalar**

   ![Extensão do Adobe Analytics](assets/collect-data-analytics/analytics-catalog-install.png)

1. Em **Gerenciamento de biblioteca** > **Conjuntos de relatórios**, insira as IDs do conjunto de relatórios que você deseja usar com cada ambiente do Launch.

   ![Insira as IDs do conjunto de relatórios](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > Não há problema em usar um conjunto de relatórios para todos os ambientes neste tutorial, mas na vida real você pode usar conjuntos de relatórios separados, como mostrado na imagem abaixo

   >[!TIP]
   >
   >Recomendamos usar a opção *Gerenciar a biblioteca para mim* como a configuração Gerenciamento de biblioteca , pois facilita muito manter a biblioteca `AppMeasurement.js` atualizada.

1. Marque a caixa para ativar **Use Activity Map**.

   ![Ativar o Activity Map de uso](assets/track-clicked-component/analytic-track-click.png)

1. Em **General** > **Servidor de rastreamento**, digite o servidor de rastreamento, por exemplo `tmd.sc.omtrdc.net`. Insira seu servidor de rastreamento SSL se o site suporta `https://`

   ![Insira os servidores de rastreamento](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Clique em **Save** para salvar as alterações.

## Adicionar uma condição à regra Página carregada

Em seguida, atualize a regra **Página carregada** para usar o elemento de dados **Tipo de recurso do componente** para garantir que a regra só seja acionada quando o evento `cmp:show` for para a **Página**. Outros componentes podem acionar o evento `cmp:show`, por exemplo, o componente Carrossel o acionará quando os slides forem alterados. Portanto, é importante adicionar uma condição para essa regra.

1. Na interface do usuário do Launch, navegue até a regra **Página carregada** criada anteriormente.
1. Em **Conditions** clique em **Add** para abrir o assistente **Condition Configuration**.
1. Para **Tipo de condição** selecione **Comparação de valor**.
1. Defina o primeiro valor no campo de formulário como `%Component Resource Type%`. Você pode usar o ícone Elemento de dados ![ícone de elemento de dados](assets/collect-data-analytics/cylinder-icon.png) para selecionar o elemento de dados **Tipo de recurso de componente**. Deixe o comparador definido como `Equals`.
1. Defina o segundo valor como `wknd/components/page`.

   ![Configuração de condição para a regra de página carregada](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > É possível adicionar essa condição na função de código personalizado que escuta o evento `cmp:show` criado anteriormente no tutorial. No entanto, adicioná-la na interface do usuário dá mais visibilidade a usuários adicionais que podem precisar fazer alterações na regra. Além disso, usamos nosso elemento de dados!

1. Salve as alterações.

## Definir variáveis do Analytics e acionar o sinal de Exibição de página

Atualmente, a regra **Página carregada** simplesmente gera uma instrução de console. Em seguida, use os elementos de dados e a extensão do Analytics para definir as variáveis do Analytics como uma **ação** na regra **Página carregada**. Também definiremos uma ação adicional para acionar o **Beacon de exibição de página** e enviar os dados coletados para o Adobe Analytics.

1. Na regra **Página carregada** **remover** a ação **Principal - Código personalizado** (as instruções do console):

   ![Remover ação de código personalizado](assets/collect-data-analytics/remove-console-statements.png)

1. Em Ações, clique em **Adicionar** para adicionar uma nova ação.
1. Defina o tipo **Extension** como **Adobe Analytics** e defina o **Tipo de ação** como **Definir variáveis**

   ![Definir a extensão de ação para definir variáveis do Analytics](assets/collect-data-analytics/analytics-set-variables-action.png)

1. No painel principal, selecione um **eVar** disponível e defina como o valor do Elemento de dados **Modelo de página**. Use o ícone Elementos de dados ![ícone Elementos de dados](assets/collect-data-analytics/cylinder-icon.png) para selecionar o elemento **Modelo de página**.

   ![Definir como modelo de página de eVar](assets/collect-data-analytics/set-evar-page-template.png)

1. Role para baixo, em **Configurações adicionais** defina **Nome da página** para o elemento de dados **Nome da página**:

   ![Nome da página Conjunto da variável de ambiente](assets/collect-data-analytics/page-name-env-variable-set.png)

   Salve as alterações.

1. Em seguida, adicione uma Ação adicional à direita do **Adobe Analytics - Definir variáveis** tocando no ícone **mais**:

   ![Adicionar uma ação de lançamento adicional](assets/collect-data-analytics/add-additional-launch-action.png)

1. Defina o tipo **Extension** como **Adobe Analytics** e defina o **Action Type** como **Send Beacon**. Como isso é considerado uma exibição de página, deixe o rastreamento padrão definido como **`s.t()`**.

   ![Ação Enviar beacon Adobe Analytics](assets/track-clicked-component/send-page-view-beacon-config.png)

1. Salve as alterações. A regra **Página carregada** agora deve ter a seguinte configuração:

   ![Configuração final de inicialização](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** Escute o  `cmp:show` evento.
   * **2.** Verifique se o evento foi acionado por uma página.
   * **3.** Definir variáveis do Analytics para Nome  **de página e Modelo**   **de página**
   * **4.** Enviar o sinal de Exibição de página do Analytics
1. Salve todas as alterações e crie a biblioteca do Launch, promovendo para o ambiente apropriado.

## Validar o sinal de Exibição de página e a chamada do Analytics

Agora que a regra **Página carregada** envia o sinal do Analytics, você deve conseguir ver as variáveis de rastreamento do Analytics usando o Experience Platform Debugger.

1. Abra o [Site WKND](https://wknd.site/us/en.html) no seu navegador.
1. Clique no ícone do Debugger ![ícone do Experience Platform Debugger](assets/collect-data-analytics/experience-cloud-debugger.png) para abrir o Experience Platform Debugger.
1. Certifique-se de que o Debugger esteja mapeando a propriedade do Launch para o *ambiente de desenvolvimento*, conforme descrito anteriormente, e **Logon do console** estiver marcado.
1. Abra o menu do Analytics e verifique se o conjunto de relatórios está definido como *seu* conjunto de relatórios. O Nome da página também deve ser preenchido:

   ![Depurador de guia do Analytics](assets/collect-data-analytics/analytics-tab-debugger.png)

1. Role para baixo e expanda **Solicitações de rede**. Você deve encontrar o **evar** definido para o **Modelo de página**:

   ![Evar e Nome da Página definidos](assets/collect-data-analytics/evar-page-name-set.png)

1. Retorne ao navegador e abra o console do desenvolvedor. Clique no **Carrossel** na parte superior da página.

   ![Clique na página do carrossel](assets/collect-data-analytics/click-carousel-page.png)

1. Observe no console do navegador a declaração do console:

   ![Condição não atendida](assets/collect-data-analytics/condition-not-met.png)

   Isso ocorre porque o carrossel aciona um evento `cmp:show` *mas* devido à verificação do **Tipo de recurso do componente**, nenhum evento é disparado.

   >[!NOTE]
   >
   > Se você não vir nenhum log de console, verifique se **Console Logging** está marcado em **Launch** no Experience Platform Debugger.

1. Navegue até uma página de artigo como [Austrália Ocidental](https://wknd.site/us/en/magazine/western-australia.html). Observe que Nome da página e Tipo de modelo são alterados.

## Parabéns!

Você acabou de usar a Adobe Client Data Layer e a Experience Platform Launch para coletar dados de página de dados de um Site de AEM e enviá-los para a Adobe Analytics.

### Próximas etapas

Consulte o tutorial a seguir para saber como usar a camada de Dados do cliente do Adobe orientada por eventos para [rastrear cliques de componentes específicos em um site do Adobe Experience Manager](track-clicked-component.md).
