---
title: Configurar o Asset Insights com a AEM Assets e tags
description: Nesta série de vídeos de cinco partes, vamos analisar a instalação e configuração do Asset Insights para Experience Manager implantado por meio de tags.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Assets as a Cloud Service, AEM Assets 6.5" before-title="false"
doc-type: Tutorial
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
duration: 2051
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# Configurar o Asset Insights com a AEM Assets e tags

Nesta série de vídeos de cinco partes, vamos analisar a instalação e configuração do Asset Insights para Experience Manager implantado por meio de tags.

## Parte 1: Visão geral do Asset Insights {#overview}

Visão geral do Asset Insights. Instale os Componentes principais, Componente de imagem de amostra e outros pacotes de conteúdo para preparar seu ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### Diagrama da arquitetura {#architecture-diagram}

![Diagrama da arquitetura](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Certifique-se de baixar o [versão mais recente dos Componentes principais](https://github.com/adobe/aem-core-wcm-components) para sua implementação.

O vídeo usa os Componentes principais v2.2.2, que não é a versão mais recente. Certifique-se de usar a versão mais recente antes de prosseguir para a próxima seção.

* Baixar [Conteúdo da imagem de amostra do Asset Insights](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Baixar [os componentes principais WCM do AEM mais recentes](https://github.com/adobe/aem-core-wcm-components/releases)

## Parte 2: Ativar o rastreamento do Asset Insights para o componente de imagem de amostra {#sample-image-component-asset-insights}

Aprimoramentos nos Componentes principais e uso do componente proxy (Componente de imagem de amostra) para o Asset Insights. Editar as políticas do modelo da página de conteúdo para ativar o componente de imagem de amostra para o site de referência.

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>O componente principal de Imagem inclui a capacidade de desativar o rastreamento de UUID ao desativar o rastreamento da UUID do ativo (valor do identificador exclusivo para um nó criado no JCR)

Usos do componente de Imagem principal ***data-asset-id*** atributo dentro do pai &lt;div> de uma tag de imagem para ativar/desativar esse recurso. O componente proxy substitui o componente principal pelas alterações a seguir.

* Remove o ***data-asset-id*** da div principal de um elemento &lt;img> dentro do image.html
* Adiciona ***data-aem-asset-id*** diretamente para o elemento &lt;img> dentro da image.html
* Adiciona ***data-trackable=&#39;true&#39;*** para o elemento &lt;img> dentro do image.html
* ***data-aem-asset-id*** e ***data-trackable=&#39;true&#39;*** são mantidas no mesmo nível de nó

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* e *data-trackable=&#39;true&#39;* são os principais atributos que precisam estar presentes para as impressões do ativo. Para o Asset Click Insights, além dos atributos de dados acima presentes na tag &lt;img> , a tag principal deve ter um valor href válido.

## Parte 3: Adobe Analytics — Criação de um conjunto de relatórios, permitindo a coleta de dados em tempo real e a geração de relatórios do AEM Assets {#adobe-analytics-asset-insights}

O conjunto de relatórios com a coleta de dados em tempo real é criado para o rastreamento de ativos. A configuração do AEM Assets Insights é definida usando credenciais do Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
>
A coleta de dados em tempo real e o relatório de ativos do AEM devem ser ativados para o conjunto de relatórios do Adobe Analytics. A habilitação do Relatório de ativos AEM reserva variáveis de análise para rastrear insights de ativos.

Para a configuração do AEM Assets Insights, você precisa das seguintes credenciais

* Centro de dados
* Nome da empresa do Analytics
* Nome de usuário do Analytics
* Segredo compartilhado (pode ser obtido em *Adobe Analytics > Administração > Configurações da empresa > Serviço da Web*).
* Conjunto de relatórios (certifique-se de selecionar o Conjunto de relatórios correto que é usado para os Relatórios de ativos)

## Parte 4: Uso de tags para adicionar a extensão do Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adição de extensão do Adobe Analytics, criação de regras de carregamento de página e integração do AEM com tags à conta técnica do Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
>
Certifique-se de replicar todas as alterações da instância do autor para a instância de publicação.

### Regra 1: Rastreador de páginas (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

O rastreador de páginas implementa dois retornos de chamada (registrados no código incorporado do ativo)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : chamado quando o evento &quot;load&quot; é despachado para o elemento asset-DOM.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : chamado quando o evento &#39;click&#39; é despachado para o asset-DOM-element. Isso é relevante somente quando o asset-DOM-element tem uma tag âncora como pai com um atributo &#39;href&#39; externo válido

Finalmente, o Rastreador de páginas implementa uma função de inicialização como.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : chamado para inicializar o componente Rastreador de páginas. Isso DEVE ser chamado antes que qualquer um dos eventos de insights do ativo (impressões e/ou cliques) seja gerado na página da Web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : opcionalmente aceita um objeto do AppMeasurement — se fornecido, ele não tentará criar uma instância do objeto do AppMeasurement.

### Regra 2: Rastreador de imagens — Ação 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### Regra 2: Rastreador de imagens — Ação 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() : é chamado no carregamento da página concluído e aciona Impressões de ativos para todas as imagens rastreáveis
* Variável do Analytics que carrega a lista de ativos carregada: **contextData[&quot;c.a.assets.idList&quot;]**
* assetAnalytics.core.assetClicked() : é chamado quando o elemento DOM do ativo tem uma tag âncora com valor href válido. Quando um ativo é clicado, um cookie é criado com a ID do ativo clicada como seu valor.**(Nome do cookie: a.assets.clickedid)**
* Variável do Analytics que carrega a lista de ativos carregada: **contextData[&#39;c.a.assets.clickedid&#39;]**
* Origem : **contextData[&#39;c.a.assets.source&#39;]**

### Instruções de depuração do console {#console-debug-statements}

```javascript
// Tags build info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JavaScript Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

Duas extensões de navegador do Google Chrome são mencionadas no vídeo como maneiras de depurar o Analytics. Extensões semelhantes também estão disponíveis para outros navegadores.

* [Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)


## Parte 5: Teste de rastreamento analítico e sincronização de dados de insight{#analytics-tracking-asset-insights}

Configuração do agendador de trabalhos de sincronização de relatórios de ativos AEM e do relatório de insights dos ativos

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
