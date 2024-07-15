---
title: Extensão modal da interface do usuário do AEM
description: Saiba como criar um modal de extensão da interface do usuário do AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Extensão modal

![Extensão modal da interface do usuário do AEM](./assets/modal/modal.png){align="center"}

O modal de extensão da interface do usuário do AEM fornece uma maneira de anexar a interface do usuário personalizada a extensões da interface do usuário do AEM.

Os modais são aplicativos do React, com base no [Espectro do React](https://react-spectrum.adobe.com/react-spectrum/), e podem criar qualquer interface personalizada exigida pela extensão, incluindo, mas não limitado a:

+ Caixas de diálogo de confirmação
+ [Formulários de entrada](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicadores de progresso](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Resumo dos resultados](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Mensagens de erro
+ ... ou até mesmo um aplicativo React completo e com múltiplas visualizações!

## Rotas modais

A experiência modal é definida pela extensão do aplicativo App Builder React definida na pasta `web-src`. Como em qualquer aplicativo React, a experiência completa é orquestrada usando [rotas React](https://reactrouter.com/en/main/components/routes) que renderizam [componentes React](https://reactjs.org/docs/components-and-props.html).

Pelo menos uma rota é necessária para gerar a exibição modal inicial. Esta rota inicial é invocada na função `onClick(..)` do [registro de extensão](#extension-registration), conforme mostrado abaixo.


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registro de extensão

Para abrir uma modal, uma chamada para `guestConnection.host.modal.showUrl(..)` é feita a partir da função `onClick(..)` da extensão. `showUrl(..)` recebeu um objeto JavaScript com chave/valores:

+ `title` fornece o nome do título do modal exibido ao usuário
+ `url` é a URL que invoca a [Rota de reação](#modal-routes) responsável pela exibição inicial do modal.

É imperativo que o `url` passado para `guestConnection.host.modal.showUrl(..)` resolva rotear na extensão, caso contrário nada será exibido no modal.

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## Componente modal

Cada rota da extensão, [que não seja a `index` rota](./extension-registration.md#app-routes), é mapeada para um componente React que pode ser renderizado no modal da extensão.

Um modal pode ser composto de qualquer número de rotas React, de um modal simples de uma rota para um modal complexo de várias rotas.

A seguir está uma ilustração de um modal de uma rota simples, no entanto, essa visualização modal pode conter links React que chamam outras rotas ou comportamentos.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## Fechar o modal

![Botão de fechamento modal da extensão da interface do usuário do AEM](./assets/modal/close.png){align="center"}

Os modais devem fornecer seu próprio controle de fechamento. Isso é feito chamando `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
