---
title: Registro da extensão do console Fragmento de conteúdo AEM
description: Saiba como registrar extensões do console Fragmento de conteúdo .
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# Registro de extensão

AEM as extensões do console do Fragmento de conteúdo são aplicativos especializados do App Builder, com base no React e usa o [Espectro React](https://react-spectrum.adobe.com/react-spectrum/) Estrutura da interface do usuário.

Para definir onde e como a extensão do console do Fragmento de conteúdo AEM aparece, duas configurações específicas são necessárias no aplicativo App Builder da extensão: roteamento de aplicativos e registro de extensão.

## Rotas do aplicativo{#app-routes}

A extensão do `App.js` declara que [Reagir roteador](https://reactrouter.com/en/main) que inclui uma rota de índice que registra a extensão no Console de fragmento de conteúdo AEM.

A rota de índice é invocada quando AEM Console do Fragmento de Conteúdo é carregado inicialmente, e o destino dessa rota define como a extensão é exposta no console.

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

`ExtensionRegistration.js` deve ser carregado imediatamente por meio da rota de índice da extensão e atua no ponto de registro da extensão, definindo:

1. O tipo de extensão; a [menu de cabeçalho](./header-menu.md) ou [barra de ações](./action-bar.md) botão.
   + [Menu Cabeçalho](./header-menu.md#extension-registration) as extensões são indicadas pela variável `headerMenu` propriedade sob `methods`.
   + [Barra de ação](./action-bar.md#extension-registration) as extensões são indicadas pela variável `actionBar` propriedade sob `methods`.
1. A definição do botão de extensão, em `getButton()` . Essa função retorna um objeto com campos:
   + `id` é uma ID exclusiva para o botão
   + `label` é o rótulo do botão de extensão no console Fragmento de conteúdo do AEM
   + `icon` é o ícone do botão de extensão no console AEM Fragmento do conteúdo . O ícone é um [Espectro React](https://spectrum.adobe.com/page/icons/) nome do ícone, com espaços removidos.
1. O manipulador de cliques do botão, em um `onClick()` .
   + [Menu Cabeçalho](./header-menu.md#extension-registration) extensões não passam parâmetros para o manipulador de cliques.
   + [Barra de ação](./action-bar.md#extension-registration) as extensões fornecem uma lista de caminhos de fragmento de conteúdo selecionados na `selections` parâmetro.

### Extensão do Menu Cabeçalho

![Extensão do Menu Cabeçalho](./assets/extension-registration/header-menu.png)

Os botões de extensão do Menu de cabeçalho são exibidos quando nenhum Fragmento de conteúdo é selecionado. Como as extensões do Menu de Cabeçalho não atuam em uma seleção de Fragmento de Conteúdo, nenhum Fragmento de Conteúdo é fornecido à sua `onClick()` manipulador.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pule para criar uma extensão de menu de cabeçalho</p>
      <p class="has-text-blackest">Saiba como registrar e definir uma extensão de menu de cabeçalho no console Fragmentos de conteúdo do AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Saiba como criar uma extensão de menu de cabeçalho">Saiba como criar uma extensão de menu de cabeçalho</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Extensão da Barra de ação

![Extensão da Barra de ação](./assets/extension-registration/action-bar.png)

Os botões de extensão da Barra de ação são exibidos quando um ou mais Fragmentos de conteúdo são selecionados. Os caminhos do Fragmento de conteúdo selecionado são disponibilizados para a extensão por meio do `selections` , no `onClick(..)` manipulador.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Ir para criar uma extensão de barra de ação</p>
      <p class="has-text-blackest">Saiba como registrar e definir uma extensão de barra de ação no console Fragmentos de conteúdo AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Saiba como criar uma extensão da barra de ações">Saiba como criar uma extensão da barra de ações</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Incluir extensões condicionalmente

AEM as extensões do Console do fragmento do conteúdo podem executar lógica personalizada para limitar quando a extensão aparece no Console do fragmento do conteúdo AEM. Essa verificação é realizada antes da `register` na chamada de `ExtensionRegistration` e retorna imediatamente se a extensão não deve ser exibida.

Esta verificação tem contexto limitado disponível:

+ O host AEM no qual a extensão está sendo carregada.
+ O token de acesso AEM do usuário atual.

As verificações mais comuns para carregar uma extensão são:

+ Usar o host AEM (`new URLSearchParams(window.location.search).get('repo')`) para determinar se a extensão deve ser carregada.
   + Mostrar somente a extensão em ambientes AEM que fazem parte de um programa específico (como mostrado no exemplo abaixo).
   + Mostrar somente a extensão em um ambiente AEM específico (ou seja, AEM host).
+ Uso de um [Ação do Adobe I/O Runtime](./runtime-action.md) para fazer uma chamada HTTP para AEM para determinar se o usuário atual deve ver a extensão.

O exemplo abaixo ilustra a limitação da extensão para todos os ambientes no programa `p12345`.

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
