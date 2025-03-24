---
title: Registro da extensão da interface do usuário do AEM
description: Saiba como registrar uma extensão da interface do usuário do AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 85
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Registro de extensão

As extensões da interface do usuário do AEM são aplicativos especializados do App Builder, com base no React e usam a estrutura da interface do usuário [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/).

Para definir onde e como a extensão da interface do usuário do AEM é exibida, duas configurações são necessárias no aplicativo App Builder da extensão: roteamento de aplicativos e o registro da extensão.

## Rotas de aplicativo{#app-routes}

A extensão `App.js` declara o [roteador React](https://reactrouter.com/en/main) que inclui uma rota de índice que registra a extensão na interface do usuário do AEM.

A rota de índice é invocada quando a interface do usuário do AEM é carregada inicialmente, e o destino dessa rota define como a extensão é exposta no console.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registro de extensão

`ExtensionRegistration.js` deve ser carregado imediatamente por meio da rota de índice da extensão e atua como ponto de registro da extensão.

Com base no modelo de extensão da interface do usuário do AEM selecionado ao [inicializar a extensão do aplicativo App Builder](./app-initialization.md), há suporte para diferentes pontos de extensão.

+ [Pontos de extensão da interface do usuário de fragmentos de conteúdo](./content-fragments/overview.md#extension-points)

## Incluir extensões condicionalmente

As extensões da interface do usuário do AEM podem executar uma lógica personalizada para limitar os ambientes AEM em que a extensão aparece. Essa verificação é executada antes da chamada `register` no componente `ExtensionRegistration` e retorna imediatamente se a extensão não deve ser exibida.

Esta verificação tem contexto limitado disponível:

+ O host do AEM no qual a extensão está sendo carregada.
+ O token de acesso AEM do usuário atual.

As verificações mais comuns para carregar uma extensão são:

+ Usando o host do AEM (`new URLSearchParams(window.location.search).get('repo')`) para determinar se a extensão deve ser carregada.
   + Mostre a extensão somente em ambientes AEM que fazem parte de um programa específico (como mostrado no exemplo abaixo).
   + Mostrar a extensão somente em um ambiente AEM específico (host do AEM).
+ Usar uma [ação do Adobe I/O Runtime](./runtime-action.md) para fazer uma chamada HTTP para o AEM para determinar se o usuário atual deve ver a extensão.

O exemplo abaixo ilustra a limitação da extensão a todos os ambientes no programa `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```
