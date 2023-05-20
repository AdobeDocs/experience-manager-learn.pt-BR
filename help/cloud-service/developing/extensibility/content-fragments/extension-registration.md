---
title: Registro da extensão do console de Fragmento de conteúdo do AEM
description: Saiba como registrar extensões do console de Fragmentos de conteúdo.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# Registro de extensão

As extensões do Console de fragmentos de conteúdo do AEM são um aplicativo especializado do App Builder, com base no React e usa o [Espectro React](https://react-spectrum.adobe.com/react-spectrum/) Estrutura da interface.

Para definir onde e como o Console do Fragmento de conteúdo do AEM é exibido na extensão, duas configurações específicas são necessárias no aplicativo App Builder da extensão: roteamento de aplicativos e o registro da extensão.

## Rotas de aplicativo{#app-routes}

A extensão do `App.js` declara o [Roteador React](https://reactrouter.com/en/main) que inclui uma rota de índice que registra a extensão no Console do fragmento de conteúdo do AEM.

A rota de índice é chamada quando o Console de fragmentos de conteúdo do AEM é carregado inicialmente, e o target dessa rota define como a extensão é exposta no console.

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

`ExtensionRegistration.js` deve ser carregado imediatamente por meio da rota de índice da extensão e atua como o ponto de registro da extensão, definindo:

1. O tipo de extensão; um [menu de cabeçalho](./header-menu.md) ou [barra de ação](./action-bar.md) botão.
   + [Menu de cabeçalho](./header-menu.md#extension-registration) as extensões são indicadas pela variável `headerMenu` propriedade em `methods`.
   + [Barra de ação](./action-bar.md#extension-registration) as extensões são indicadas pela variável `actionBar` propriedade em `methods`.
1. A definição do botão de extensão, em `getButton()` função. Esta função retorna um objeto com campos:
   + `id` é um identificador exclusivo do botão
   + `label` é o rótulo do botão de extensão no console de Fragmentos de conteúdo do AEM
   + `icon` é o ícone do botão de extensão no console de Fragmentos de conteúdo do AEM. O ícone é um [Espectro React](https://spectrum.adobe.com/page/icons/) nome do ícone, com espaços removidos.
1. O manipulador de cliques do botão, em definido em uma `onClick()` função.
   + [Menu de cabeçalho](./header-menu.md#extension-registration) As extensões não transmitem parâmetros para o manipulador de cliques.
   + [Barra de ação](./action-bar.md#extension-registration) As extensões fornecem uma lista dos caminhos de fragmento de conteúdo selecionados na `selections` parâmetro.

### Extensão do menu Cabeçalho

![Extensão do menu Cabeçalho](./assets/extension-registration/header-menu.png)

Os botões de extensão do Menu de cabeçalho são exibidos quando nenhum Fragmento de conteúdo é selecionado. Como as extensões do Menu de cabeçalho não agem em uma seleção de Fragmento de conteúdo, nenhum Fragmento de conteúdo é fornecido a `onClick()` manipulador.

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pular para a criação de uma extensão de menu de cabeçalho</p>
      <p class="has-text-blackest">Saiba como registrar e definir uma extensão de menu de cabeçalho no Console de fragmentos de conteúdo do AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Saiba como criar uma extensão de menu de cabeçalho">Saiba como criar uma extensão de menu de cabeçalho</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Extensão da Barra de Ações

![Extensão da Barra de Ações](./assets/extension-registration/action-bar.png)

Os botões de extensão da Barra de ações são exibidos quando um ou mais Fragmentos de conteúdo são selecionados. Os caminhos do fragmento de conteúdo selecionado são disponibilizados para a extensão por meio do `selections` parâmetro, no campo do botão `onClick(..)` manipulador.

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pular para criar uma extensão da barra de ações</p>
      <p class="has-text-blackest">Saiba como registrar e definir uma extensão de barra de ação no Console de fragmentos de conteúdo do AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Saiba como criar uma extensão da barra de ações">Saiba como criar uma extensão da barra de ações</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Incluir extensões condicionalmente

As extensões do Console de fragmentos de conteúdo do AEM podem executar uma lógica personalizada para limitar quando a extensão aparece no Console de fragmentos de conteúdo do AEM. Essa verificação é realizada antes da variável `register` chame no `ExtensionRegistration` e retornará imediatamente se a extensão não for exibida.

Esta verificação tem contexto limitado disponível:

+ O host AEM no qual a extensão está carregando.
+ O token de acesso AEM do usuário atual.

As verificações mais comuns para carregar uma extensão são:

+ Utilização do host AEM (`new URLSearchParams(window.location.search).get('repo')`) para determinar se a extensão deve ser carregada.
   + Mostrar apenas a extensão em ambientes AEM que fazem parte de um programa específico (como mostrado no exemplo abaixo).
   + Mostrar somente a extensão em um ambiente AEM específico (ou seja, host AEM).
+ Uso de um [Ação do Adobe I/O Runtime](./runtime-action.md) para fazer uma chamada HTTP ao AEM a fim de determinar se o usuário atual deve ver a extensão.

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
