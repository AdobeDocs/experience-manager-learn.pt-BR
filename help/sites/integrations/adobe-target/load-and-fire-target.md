---
title: Carregar e acionar uma chamada do Target
description: Saiba como carregar, transmitir parâmetros para solicitação de página e acionar uma chamada do Target na página do site usando uma regra de tags.
feature: Core Components, Adobe Client Data Layer
version: Experience Manager as a Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 588
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 1%

---

# Carregar e acionar uma chamada do Target {#load-fire-target}

Saiba como carregar, transmitir parâmetros para solicitação de página e acionar uma chamada do Target na página do site usando uma regra de tags. As informações da página da Web são recuperadas e passadas como parâmetros usando a Camada de dados do cliente da Adobe, que permite coletar e armazenar dados sobre a experiência do visitante em uma página da Web e, em seguida, facilitar o acesso a esses dados.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regra de carregamento de página

A Camada de dados de clientes Adobe é uma camada de dados orientada por eventos. Quando a camada de dados da Página do AEM é carregada, ela aciona um evento `cmp:show`. No vídeo, a regra `tags Library Loaded` é invocada usando um evento personalizado. Abaixo, você pode encontrar os trechos de código usados no vídeo para o evento personalizado e para os elementos de dados.

### Evento de exibição de página personalizada{#page-event}

![Configuração de evento mostrada na página e código personalizado](assets/load-and-fire-target-call.png)

Na propriedade de marcas, adicione um novo **Evento** à **Regra**

+ __Extensão:__ Principal
+ __Tipo de evento:__ código personalizado
+ __Nome:__ Manipulador de eventos de Exibição de Página (ou algo descritivo)

Toque no botão __Abrir Editor__ e cole no seguinte trecho de código. Este código __deve__ ser adicionado à __Configuração do Evento__ e a uma __Ação__ subsequente.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Uma função personalizada define o `pageShownEventHandler` e acompanha eventos emitidos pelos Componentes Principais do AEM, deriva as informações relevantes do Componente Principal, empacota-o em um objeto de evento e aciona o Evento de tags com as informações do evento derivado em sua carga.

A Regra de marcas é disparada usando a função `trigger(...)` das marcas, que é __only__ disponível em uma definição de trecho de código Personalizado do Evento de Regra.

A função `trigger(...)` usa um objeto de evento como um parâmetro que, por sua vez, é exposto em Elementos de Dados de marcas por outro nome reservado em marcas chamadas `event`. Agora os elementos de dados nas marcas podem fazer referência a dados desse objeto de evento do objeto `event` usando uma sintaxe como `event.component['someKey']`.

Se `trigger(...)` for usado fora do contexto de um tipo de evento de Código personalizado do evento (por exemplo, em uma Ação), o erro de JavaScript `trigger is undefined` será lançado no site integrado à propriedade de marcas.


### Elementos de dados

![Elementos de dados](assets/data-elements.png)

Os elementos de dados de marcas mapeiam os dados do objeto de evento [acionado no evento personalizado Página exibida](#page-event) para variáveis disponíveis no Adobe Target, por meio do Tipo de elemento de dados Código personalizado da extensão principal.

#### Elemento de dados da ID da página

```
if (event && event.id) {
    return event.id;
}
```

Esse código retorna a ID exclusiva gerada pelo Componente principal.

![ID da Página](assets/pageid.png)

### Elemento de dados do caminho da página

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Esse código retorna o caminho da página do AEM.

![Caminho da página](assets/pagepath.png)

### Elemento de dados do título da página

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Esse código retorna o título da página do AEM.

![Título da página](assets/pagetitle.png)

## Resolução de problemas

### Por que as mboxes não estão sendo acionadas nas minhas páginas da Web?

#### Mensagem de erro quando o cookie mboxDisable não está definido

![Erro de domínio do cookie de destino](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Solução

Os clientes do, às vezes, usam instâncias baseadas em nuvem com o Target para testes ou fins de prova de conceito simples. Esses domínios e muitos outros fazem parte da Lista de sufixos públicos.
Se estiver usando esses domínios, os navegadores modernos não salvarão os cookies, a menos que você personalize a configuração de `cookieDomain` usando `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Próximas etapas

+ [Exportar fragmento de experiência para o Adobe Target](./export-experience-fragment-target.md)

## Links de suporte

+ [Documentação da Camada de Dados de Clientes Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Usando a Camada de Dados de Clientes Adobe e a Documentação de Componentes Principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR)
+ [Introdução ao Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
