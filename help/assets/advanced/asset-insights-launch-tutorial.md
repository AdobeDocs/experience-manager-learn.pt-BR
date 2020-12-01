---
title: Configurar insights de ativos com AEM Assets e lançamento de Adobe
description: Nesta série de vídeo de 5 partes, percorremos a configuração do Asset Insights para o Experience Manager implantado via Launch by Adobe.
contentOwner: selvaraj
feature: asset-insights
topics: integrations, development, metadata
audience: developer, architect, administrator
doc-type: article
activity: implement
version: 6.3, 6.4, 6.5
redirect-form: https://docs.adobe.com/content/help/en/experience-manager-learn/assets/analytics/asset-insights-launch-tutorial-setup.html
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---


# Configurar insights de ativos com a AEM Assets e a Adobe Experience Platform Launch

Nesta série de vídeos de 5 partes, percorremos a configuração e a configuração do Asset Insights para o Experience Manager implantado por meio do Adobe Launch.

## Parte 1: Visão geral do Asset Insights {#overview}

Visão geral do Asset Insights. Instale os componentes principais, o componente de imagem de amostra e outros pacotes de conteúdo para preparar o ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### Diagrama de arquitetura {#architecture-diagram}

![Diagrama de arquitetura](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Certifique-se de baixar a versão mais recente [dos Componentes principais](https://github.com/adobe/aem-core-wcm-components) para sua implementação.

O vídeo usa Componentes principais v2.2.2 que não são mais a versão mais recente; utilize a versão mais recente antes de prosseguir para a próxima seção.

* Baixar [Conteúdo da imagem de amostra do Asset Insights](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Baixe [os componentes principais mais recentes do AEM WCM](https://github.com/adobe/aem-core-wcm-components/releases)

## Parte 2 : Ativando o rastreamento de insights de ativos para o componente de imagem de amostra {#sample-image-component-asset-insights}

Aprimoramentos aos componentes principais e uso do componente proxy (Componente de imagem de amostra) para o Asset Insights. Editar as políticas de modelo de página de conteúdo para ativar o componente de imagem de amostra para o site de referência.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>O Componente principal da imagem inclui a capacidade de desativar o rastreamento UUID desativando o rastreamento do UUID do ativo (valor identificador exclusivo para um nó criado no JCR)

O componente Imagem principal usa o atributo ***data-asset-id*** dentro do pai &lt;div> de uma tag de imagem para ativar/desativar esse recurso. O componente proxy substitui o componente principal pelas seguintes alterações.

* Remove o ***data-asset-id*** do div pai de um elemento &lt;img> dentro do image.html
* Adiciona ***data-aem-asset-id*** diretamente ao elemento &lt;img> dentro de image.html
* Adiciona o valor ***data-trackable=&#39;true&#39;*** ao elemento &lt;img> dentro de image.html
* ***data-aem-asset-*** idand  ***data-trackable=&#39;true&#39;*** são mantidos no mesmo nível de nó

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* e  *data-trackable=&#39;true&#39;* são os principais atributos que precisam estar presentes para impressões de ativos. Para Insights de cliques de ativos, além dos atributos de dados acima presentes na tag &lt;img>, a tag pai &lt;a> deve ter um valor href válido.

## Parte 3: Adobe Analytics — Criação do conjunto de relatórios, habilitando a coleta de dados em tempo real e o Relatórios AEM Assets {#adobe-analytics-asset-insights}

O conjunto de relatórios com coleta de dados em tempo real é criado para o rastreamento de ativos. A configuração do AEM Assets Insights é configurada usando as credenciais do Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
A coleta de dados em tempo real e o Relatórios de ativos AEM precisam ser habilitados para seu conjunto de relatórios Adobe Analytics. Habilitar AEM Relatórios de ativos reserva variáveis de análise para rastrear insights de ativos.

Para a configuração do AEM Assets Insights, você precisa das seguintes credenciais

* Centro de dados
* Nome da Empresa do Analytics
* Nome de usuário do Analytics
* Segredo compartilhado (pode ser obtido em *Adobe Analytics > Admin > Configurações de Empresa > Serviço Web*).
* Conjunto de relatórios (certifique-se de selecionar o conjunto de relatórios correto que é usado para o Relatórios do ativo)

## Parte 4: Uso do Adobe Experience Platform Launch para adicionar a extensão Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adicionando o Adobe Analytics Extension, Criando regras de carregamento de página e Integrando AEM com a conta técnica Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
Certifique-se de replicar todas as suas alterações da instância do autor para a instância de publicação.

### Regra 1 : Rastreador de página (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

O rastreador de páginas implementa duas chamadas de retorno (registradas no código incorporado do ativo)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>: chamado quando o evento &#39;load&#39; é despachado para o elemento asset-DOM.&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;/code>** &lt;code>&lt;code>: chamado quando o evento &#39;click&#39; é despachado para o elemento asset-DOM, isso só é relevante quando o elemento asset-DOM tem uma tag de âncora como pai com um atributo &#39;href&#39; externo válido&lt;/code>&lt;/code>

Por fim, o Pagetracker implementa uma função de inicialização como.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>: chamado para inicializar o componente Pagetracker.&lt;/code>&lt;/code> ISSO DEVE ser chamado antes que qualquer evento-insights-ativos (impressões e/ou cliques) seja gerado da página da Web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>: aceita opcionalmente um objeto AppMeasurement — se fornecido, ele não tentará criar uma nova instância do objeto AppMeasurement.&lt;/code>&lt;/code>

### Regra 2: Controlador de imagens — Ação 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### Regra 2: Controlador de imagens — Ação 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded(): é chamado quando o carregamento da página é concluído e aciona Impressões de ativos para todas as imagens rastreáveis
* Variável do Analytics que contém a lista de ativos carregados: **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() : é chamado quando o elemento DOM do ativo tem uma tag de âncora com valor href válido. Quando um ativo é clicado, um cookie é criado com a ID de ativo clicada como seu valor.**(Nome do cookie: a.assets.click-did)**
* Variável do Analytics que contém a lista de ativos carregados: **contextData[&#39;c.a.assets.clickedid&#39;]**
* Fonte da Origem : **contextData[&#39;c.a.assets.source&#39;]**

### Declarações de depuração do console {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

Duas extensões do navegador Google Chrome são mencionadas no vídeo como maneiras de depurar o Analytics. Extensões semelhantes também estão disponíveis para outros navegadores.

* [Iniciar extensão do Chrome do Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

Também é possível alternar o DTM para o modo de depuração com a seguinte Extensão do Chrome: [Iniciar e Comutador DTM](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). Isso facilita ver se há erros relacionados à implantação do DTM. Além disso, você pode alternar manualmente o DTM para o modo de depuração por meio de qualquer navegador *ferramentas do desenvolvedor -> Console JS* adicionando o seguinte trecho:

## Parte 5 : Testando o rastreamento analítico e a sincronização de dados do Insight{#analytics-tracking-asset-insights}

Configuração AEM Scheduler de Trabalho de Sincronização de Relatórios de Ativos e Relatório de Insights de Ativos

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
