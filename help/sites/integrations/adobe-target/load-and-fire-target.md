---
title: Carregar e acionar uma chamada de Público alvo
description: Saiba como carregar, passar parâmetros para a solicitação de página e acionar uma chamada de Público alvo da página do site usando uma regra de inicialização. As informações da página são recuperadas e passadas como parâmetros usando a Camada de dados do cliente Adobe que permite coletar e armazenar dados sobre a experiência do visitante em uma página da Web e, em seguida, facilitar o acesso a esses dados.
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 3%

---


# Carregar e acionar uma chamada de Público alvo {#load-fire-target}

Saiba como carregar, passar parâmetros para a solicitação de página e acionar uma chamada de Público alvo da página do site usando uma regra de inicialização. As informações da página da Web são recuperadas e passadas como parâmetros usando a Camada de dados do cliente Adobe que permite coletar e armazenar dados sobre a experiência do visitante em uma página da Web e, em seguida, facilitar o acesso a esses dados.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regra de carregamento de página

A Camada de dados do cliente Adobe é uma camada de dados orientada por evento. Quando a camada de dados da Página AEM for carregada, ela acionará um evento `cmp:show`. No vídeo, a regra `Launch Library Loaded` é invocada usando um evento personalizado. Abaixo, você pode encontrar os trechos de código usados no vídeo para o evento personalizado, bem como para os elementos de dados.

### Evento exibido na página personalizada{#page-event}

![Configuração de evento mostrada na página e código personalizado](assets/load-and-fire-target-call.png)

Na propriedade Launch, adicione um novo **Evento** à **Regra**

+ __Extensão:__ Core
+ __tipo de evento:Código__ personalizado
+ __Nome:Manipulador de Eventos de Apresentação de__ Página (ou algo descritivo)

Toque no botão __Abrir editor__ e cole no seguinte trecho de código. Este código __deve__ ser adicionado à __Configuração do Evento__ e a uma __Ação__ subsequente.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Uma função personalizada define o `pageShownEventHandler` e escuta os eventos emitidos pelos Componentes Principais AEM, deriva as informações relevantes do Componente Principal, agrupa-o em um objeto de evento e aciona o Evento Launch com as informações do evento derivado em sua carga útil.

A regra de inicialização é acionada usando a função `trigger(...)` do Launch, que é __only__ disponível na definição do trecho de código personalizado do Evento de uma regra.

A função `trigger(...)` usa um objeto de evento como parâmetro que, por sua vez, é exposto em Elementos de dados de inicialização por outro nome reservado em Iniciar chamado `event`. Os elementos de dados no Launch agora podem fazer referência aos dados desse objeto de evento do objeto `event` usando sintaxe como `event.component['someKey']`.

Se `trigger(...)` for usado fora do contexto de um tipo de evento de Código Personalizado do Evento (por exemplo, em uma Ação), o erro de JavaScript `trigger is undefined` será lançado no site integrado à propriedade Launch.


### Elementos de dados

![Elementos de dados](assets/data-elements.png)

Os Elementos de dados de inicialização do Adobe mapeiam os dados do objeto do evento [acionado no evento personalizado Página exibida](#page-event) para variáveis disponíveis no Adobe Target, por meio do Tipo de elemento de dados de código personalizado da extensão principal.

#### Elemento de dados de ID da página

```
if (event && event.id) {
    return event.id;
}
```

Esse código retorna a ID exclusiva gerada pelo Componente principal.

![ID da página](assets/pageid.png)

### Elemento de dados do caminho da página

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Esse código retorna o caminho da página AEM.

![Caminho da página](assets/pagepath.png)

### Elemento de dados do título da página

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Esse código retorna o título da página AEM.

![Título da página](assets/pagetitle.png)

## Resolução de problemas

### Por que minhas mboxes não estão sendo acionadas em minhas páginas da Web?

#### Mensagem de erro quando o cookie mboxDisable não está definido**

![Erro de domínio do cookie do público alvo](assets/target-cookie-error.png)

#### Solução

Os clientes do público alvo às vezes usam instâncias baseadas em nuvem com Público alvo para testes ou para fins simples de prova de conceito. Esses domínios, e muitos outros, fazem parte da Lista Sufixo Público .
Os navegadores modernos não salvarão cookies se você estiver usando esses domínios, a menos que personalize a configuração `cookieDomain` usando `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Próximas etapas

+ [Exportar fragmento de experiência para Adobe Target](./export-experience-fragment-target.md)

## Links de suporte

+ [Documentação da camada de dados do cliente Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Uso da camada de dados do cliente Adobe e da documentação dos componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Introdução ao Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)