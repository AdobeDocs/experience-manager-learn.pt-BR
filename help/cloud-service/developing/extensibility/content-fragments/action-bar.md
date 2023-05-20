---
title: Extensões da barra de ação do console de Fragmento de conteúdo do AEM
description: Saiba como criar extensões da barra de ação do console de Fragmento de conteúdo do AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 97d26a1f-f9a7-4e57-a5ef-8bb2f3611088
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Extensão da barra de ações

![Extensão da barra de ações](./assets/action-bar/action-bar.png){align="center"}

Extensões que incluem uma barra de ação introduzem um botão à ação do Console de fragmentos de conteúdo do AEM que é exibido quando __1 ou mais__ Fragmentos de conteúdo são selecionados. Como os botões de extensão da barra de ação são exibidos somente quando pelo menos um Fragmento de conteúdo é selecionado, eles normalmente agem de acordo com os Fragmentos de conteúdo selecionados. Os exemplos incluem:

+ Chamar um processo de negócios ou fluxo de trabalho nos Fragmentos de conteúdo selecionados.
+ Atualizar ou alterar os dados dos fragmentos de conteúdo selecionados.

## Registro de extensão

`ExtensionRegistration.js` é o ponto de entrada para a extensão AEM e define:

1. O tipo de extensão; no caso, um botão de barra de ação.
1. A definição do botão de extensão, em `getButton()` função.
1. O manipulador de cliques do botão, na caixa `onClick()` função.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## Modal

![Modal](./assets/modal/modal.png)

As extensões da barra de ação do console de fragmentos de conteúdo AEM podem exigir:

+ Entrada adicional do usuário para executar a ação desejada.
+ A capacidade de fornecer ao usuário informações detalhadas sobre o resultado da ação.

Para oferecer suporte a esses requisitos, a extensão Console do fragmento de conteúdo do AEM permite um modal personalizado que é renderizado como um aplicativo React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pular para a criação de um modal</p>
      <p class="has-text-blackest">Saiba como criar uma modal exibida ao clicar no botão de extensão da barra de ações.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Saiba como criar um modal">Saiba como criar um modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Sem modal

Ocasionalmente, as extensões da Barra de ação do console Fragmento de conteúdo do AEM não exigem mais interação com o usuário, por exemplo:

+ Chamar um processo de back-end que não requer entrada do usuário, como importação ou exportação.

Nesses casos, a extensão do console Fragmento de conteúdo do AEM não requer uma [modal](#modal)e execute o trabalho diretamente no botão da barra de ações `onClick` manipulador.

A extensão do console de Fragmentos de conteúdo do AEM permite que um indicador de progresso sobreponha o console de Fragmentos de conteúdo do AEM enquanto o trabalho está sendo executado, impedindo que o usuário execute mais ações. O uso do indicador de progresso é opcional, mas útil para comunicar o progresso do trabalho síncrono ao usuário.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
