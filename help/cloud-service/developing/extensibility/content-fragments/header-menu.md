---
title: Extensões de menu do cabeçalho do console Fragmento de conteúdo AEM
description: Saiba como criar extensões de menu de cabeçalho do console do Fragmento de conteúdo AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---


# Extensão do menu Cabeçalho

![Extensão do menu Cabeçalho](./assets/header-menu/header-menu.png){align="center"}

Extensões que incluem um menu de cabeçalho, apresentam um botão ao cabeçalho do Console do Fragmento de conteúdo AEM que é exibido quando __não__ Fragmentos de conteúdo são selecionados. Como os botões de extensão do menu de cabeçalho são exibidos somente quando nenhum Fragmento de conteúdo é selecionado, eles normalmente não atuam em Fragmentos de conteúdo existentes. Em vez disso, as extensões de menus de cabeçalho normalmente:

+ Crie novos Fragmentos de conteúdo usando a lógica personalizada, como criar um conjunto de Fragmentos de conteúdo, vinculado por referências de conteúdo.
+ Atuando em um conjunto de Fragmentos de conteúdo selecionado de forma programática, como exportar todos os Fragmentos de conteúdo criados na última semana.

## Registro de extensão

`ExtensionRegistration.js` é o ponto de entrada da extensão de AEM e define:

1. O tipo de extensão; no caso, um botão de menu de cabeçalho.
1. A definição do botão de extensão, em `getButton()` .
1. O manipulador de cliques do botão, na `onClick()` .

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## Modal

![Modal](./assets/modal/modal.png)

AEM As extensões de menu do cabeçalho do Console do fragmento de conteúdo podem exigir:

+ Entrada adicional do usuário para executar a ação desejada.
+ A capacidade de fornecer ao usuário informações detalhadas sobre o resultado da ação.

Para ser compatível com esses requisitos, a extensão Console do fragmento de conteúdo AEM permite um modal personalizado que é renderizado como um aplicativo React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pule para criar uma modal</p>
      <p class="has-text-blackest">Saiba como criar uma modal exibida ao clicar no botão de extensão do menu de cabeçalho.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Saiba como criar um modal">Saiba como criar um modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Sem modal

Ocasionalmente, AEM extensões de menu de cabeçalho do console do Fragmento de conteúdo não exigem mais interação com o usuário, por exemplo:

+ Chamar um processo de back-end que não requer entrada do usuário, como importação ou exportação.
+ Abrir uma nova página da Web, como documentação interna sobre diretrizes de conteúdo.

Nesses casos, a extensão do console Fragmento do conteúdo do AEM não requer um [modal](#modal)e pode executar o trabalho diretamente no botão do menu de cabeçalho `onClick` manipulador.

A extensão Console do fragmento de conteúdo AEM permite que um indicador de progresso sobreponha o Console do fragmento de conteúdo AEM enquanto o trabalho está sendo executado, impedindo que o usuário execute outras ações. O uso do indicador de progresso é opcional, mas útil para comunicar o progresso do trabalho síncrono ao usuário.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
