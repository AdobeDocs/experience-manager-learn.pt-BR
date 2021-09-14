---
title: Carregar e acionar uma chamada do Target
description: Saiba como carregar, passar parâmetros para solicitação de página e acionar uma chamada do Target de sua página do site usando uma regra do Launch. As informações da página são recuperadas e passadas como parâmetros usando a Camada de dados do cliente do Adobe , que permite coletar e armazenar dados sobre a experiência dos visitantes em uma página da Web e, em seguida, facilitar o acesso a esses dados.
feature: Core Components, Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 1%

---

# Carregar e acionar uma chamada do Target {#load-fire-target}

Saiba como carregar, passar parâmetros para solicitação de página e acionar uma chamada do Target de sua página do site usando uma regra do Launch. As informações da página da Web são recuperadas e passadas como parâmetros usando a Camada de dados do cliente do Adobe, que permite coletar e armazenar dados sobre a experiência dos visitantes em uma página da Web e, em seguida, facilitar o acesso a esses dados.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regra de carregamento de página

A Camada de dados do cliente do Adobe é uma camada de dados orientada por eventos. Quando a camada de dados Página de AEM é carregada, ela acionará um evento `cmp:show` . No vídeo, a regra `Launch Library Loaded` é invocada usando um evento personalizado. Abaixo, você pode encontrar os trechos de código usados no vídeo para o evento personalizado, bem como para os elementos de dados.

### Evento de página exibida personalizada{#page-event}

![Configuração de evento mostrada na página e código personalizado](assets/load-and-fire-target-call.png)

Na propriedade Launch, adicione um novo **Event** ao **Rule**

+ __Extensão:__ principal
+ __Tipo de evento:__ Código personalizado
+ __Nome:__ Manipulador de evento de exibição de página (ou algo descritivo)

Toque no botão __Abrir editor__ e cole no seguinte trecho de código. Este código __deve__ ser adicionado ao __Configuração do Evento__ e a __Ação__ subsequente.

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

Uma função personalizada define o `pageShownEventHandler` e escuta eventos emitidos pelos Componentes principais AEM, deriva as informações relevantes do Componente principal, agrupa-o em um objeto de evento e aciona o Evento de inicialização com as informações do evento derivadas na carga.

A Regra do Launch é acionada usando a função `trigger(...)` do Launch, que é __somente__ disponível na definição do trecho de código personalizado do evento de uma regra.

A função `trigger(...)` pega um objeto de evento como parâmetro que, por sua vez, é exposto em Elementos de dados do Launch, por outro nome reservado no Launch chamado `event`. Os elementos de dados no Launch agora podem fazer referência aos dados desse objeto de evento do objeto `event` usando a sintaxe como `event.component['someKey']`.

Se `trigger(...)` for usado fora do contexto do tipo de evento Código personalizado de um evento (por exemplo, em uma Ação), o erro JavaScript `trigger is undefined` será lançado no site integrado à propriedade do Launch.


### Elementos de dados

![Elementos de dados](assets/data-elements.png)

Os Elementos de dados do Adobe Launch mapeiam os dados do objeto de evento [acionado no evento personalizado Página exibida](#page-event) para variáveis disponíveis no Adobe Target, por meio do Tipo de elemento de dados de código personalizado da extensão principal.

#### Elemento de dados da ID da página

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

Esse código retorna o título AEM página.

![Título da página](assets/pagetitle.png)

## Resolução de problemas

### Por que as mboxes não estão sendo acionadas nas minhas páginas da Web?

#### Mensagem de erro quando o cookie mboxDisable não está definido

![Erro de Domínio de Cookie de Destino](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Solução

Os clientes do Target às vezes usam instâncias baseadas em nuvem com o Target para testes ou fins de prova de conceito simples. Esses domínios e muitos outros fazem parte da Lista de sufixos públicos .
Se estiver usando esses domínios, os navegadores modernos não salvarão os cookies, a menos que você personalize a configuração `cookieDomain` usando `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Próximas etapas

+ [Exportar fragmento de experiência para o Adobe Target](./export-experience-fragment-target.md)

## Links de suporte

+ [Documentação da camada de dados do cliente Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Uso da camada de dados do cliente do Adobe e da documentação dos componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Introdução ao Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)
