---
title: Configurar o Asset Insights com o AEM Assets e o Adobe Launch
description: Nesta série de vídeos de 5 partes, percorremos a configuração do Asset Insights para o Experience Manager implantado via Launch by Adobe.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 1%

---

# Configurar o Asset Insights com a AEM Assets e a Adobe Experience Platform Launch

Nesta série de vídeos de 5 partes, percorremos a configuração do Asset Insights para o Experience Manager implantado por meio do Adobe Launch.

## Parte 1: Visão geral do Asset Insights {#overview}

Visão geral do Asset Insights. Instale os Componentes principais, o Componente de imagem de amostra e outros pacotes de conteúdo para preparar o ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### Diagrama de arquitetura {#architecture-diagram}

![Diagrama de arquitetura](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Faça o download do [última versão dos Componentes principais](https://github.com/adobe/aem-core-wcm-components) para sua implementação.

O vídeo usa os Componentes principais v2.2.2, que não são mais a versão mais recente; certifique-se de usar a versão mais recente antes de prosseguir para a próxima seção.

* Baixar [Conteúdo de imagem de amostra do Asset Insights](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Baixar [os Componentes principais do WCM AEM mais recentes](https://github.com/adobe/aem-core-wcm-components/releases)

## Parte 2 : Ativar o rastreamento do Asset Insights para o componente de imagem de amostra {#sample-image-component-asset-insights}

Aprimoramentos aos Componentes principais e uso do componente proxy (Componente de imagem de exemplo) para o Asset Insights. Editar as políticas do modelo da página de conteúdo para ativar o componente de imagem de amostra para o site de referência.

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>O Componente principal de imagem inclui a capacidade de desativar o rastreamento de UUID ao desativar o rastreamento de UUID do ativo (valor identificador exclusivo para um nó criado no JCR)

O componente Imagem principal usa ***data-asset-id*** atributo dentro do pai &lt;div> de uma tag de imagem para ativar/desativar este recurso. O Componente de proxy substitui o componente principal pelas seguintes alterações.

* Remove o ***data-asset-id*** do div principal de um  &lt;img> elemento dentro de image.html
* Adicionadas ***data-aem-asset-id*** diretamente para o  &lt;img> elemento dentro de image.html
* Adicionadas ***data-trackable=&#39;true&#39;*** valor para o  &lt;img> elemento dentro de image.html
* ***data-aem-asset-id*** e ***data-trackable=&#39;true&#39;*** são mantidos no mesmo nível de nó

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* e *data-trackable=&#39;true&#39;* são os principais atributos que precisam estar presentes para Impressões de ativos. Para os Insights de cliques de ativos, além dos atributos de dados acima presentes na  &lt;img> tag , a tag principal deve ter um valor href válido.

## Parte 3: Adobe Analytics — Criar conjunto de relatórios, permitindo a coleta de dados em tempo real e os relatórios do AEM Assets {#adobe-analytics-asset-insights}

O conjunto de relatórios com coleta de dados em tempo real é criado para o rastreamento de ativos. A configuração do AEM Assets Insights é configurada usando as credenciais do Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
A coleta de dados em tempo real e AEM Relatórios de ativos precisam ser ativados para seu Conjunto de relatórios do Adobe Analytics. Ativar AEM Relatórios de ativos reserva variáveis de análise para rastrear insights de ativos.

Para a configuração do AEM Assets Insights, você precisa das seguintes credenciais

* Centro de dados
* Nome da empresa do Analytics
* Nome de usuário do Analytics
* Segredo compartilhado (pode ser obtido de *Adobe Analytics > Administrador > Configurações da empresa > Serviço da Web*).
* Report Suite (Certifique-se de selecionar o Report Suite correto que é usado para o Asset Reporting)

## Parte 4: Uso do Adobe Experience Platform Launch para adicionar a extensão Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adicionar a extensão do Adobe Analytics, criar regras de carregamento de página e integrar o AEM com o Launch com a conta técnica do Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
Certifique-se de replicar todas as alterações da instância do autor para a instância de publicação.

### Artigo 1º : Rastreador de página (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

O rastreador de página implementa duas chamadas de retorno (registradas no código incorporado do ativo)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : chamado quando o evento &#39;load&#39; é despachado para o elemento asset-DOM.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : chamado quando o evento &#39;click&#39; é despachado para o elemento asset-DOM, isso é relevante somente quando o elemento asset-DOM tem uma tag de âncora como principal com um atributo &#39;href&#39; externo válido

Por fim, o Pagetracker implementa uma função de inicialização como .

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : chamado para inicializar o componente Pagetracker . Isso DEVE ser chamado antes que qualquer um dos eventos do asset-insights (impressões e/ou cliques) seja gerado na página da Web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : aceita opcionalmente um objeto AppMeasurement — se fornecido, ele não tenta criar uma nova instância do objeto AppMeasurement.

### Regra 2: Rastreador de imagem — Ação 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### Regra 2: Rastreador de imagem — Ação 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() : é chamado quando o carregamento da página é concluído e aciona Impressões de ativos para todas as imagens rastreáveis
* Variável do Analytics que contém a lista de ativos carregados : **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() : é chamado quando o elemento DOM do ativo tem uma tag de âncora com valor href válido. Quando um ativo é clicado, um cookie é criado com a ID do ativo clicado como seu valor.**(Nome do cookie: a.assets.clickedid)**
* Variável do Analytics que contém a lista de ativos carregados : **contextData[&#39;c.a.assets.clickedid&#39;]**
* Origem : **contextData[&#39;c.a.assets.source&#39;]**

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

Duas extensões de navegador Google Chrome são mencionadas no vídeo como maneiras de depurar o Analytics. Extensões semelhantes também estão disponíveis para outros navegadores.

* [Extensão Launch Switch Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

Também é possível alternar o DTM para o modo de depuração com a seguinte Extensão do Chrome: [Launch e DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). Isso facilita ver se há erros relacionados à implantação do DTM. Além disso, é possível alternar manualmente o DTM para o modo de depuração por meio de qualquer navegador *ferramentas do desenvolvedor -> Console JS* adicionando o seguinte trecho:

## Parte 5 : Teste do rastreamento analítico e sincronização de dados de insight{#analytics-tracking-asset-insights}

Configuração AEM relatório de ativos Sincronizar o agendador de trabalhos e o relatório do Assets Insights

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
