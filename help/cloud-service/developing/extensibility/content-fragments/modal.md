---
title: AEM modal da extensão do console Fragmento de conteúdo
description: Saiba como criar um modal de extensão do console Fragmento de conteúdo AEM.
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
source-wordcount: '344'
ht-degree: 0%

---


# Modal de extensão

![AEM modal da extensão Fragmento de conteúdo](./assets/modal/modal.png){align="center"}

AEM modal da extensão Fragmento de conteúdo fornece uma maneira de anexar a interface do usuário personalizada às extensões AEM Fragmento de conteúdo, seja ela [Barra de ação](./action-bar.md) ou [Menu Cabeçalho](./header-menu.md) botões.

Os módulos são aplicativos React, com base em [Espectro React](https://react-spectrum.adobe.com/react-spectrum/)e podem criar qualquer interface de usuário personalizada exigida pela extensão, incluindo, entre outros:

+ Caixas de diálogo de confirmação
+ [Formulários de entrada](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicadores de progresso](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Resumo dos resultados](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Mensagens de erro
+ ... ou até mesmo um aplicativo React com várias visualizações e completo!

## Rotas modais

A experiência modal é definida pelo aplicativo App Builder React de extensão definido no `web-src` pasta. Assim como em qualquer aplicativo React, a experiência completa é orquestrada usando [Reagir rotas](https://reactrouter.com/en/main/components/routes) essa renderização [Reagir componentes](https://reactjs.org/docs/components-and-props.html).

Pelo menos uma rota é necessária para gerar a visualização modal inicial. Essa rota inicial é chamada na [registro de extensão](#extension-registration)&#39;s `onClick(..)` , conforme mostrado abaixo.


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
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

Para abrir um modal, faça uma chamada para `guestConnection.host.modal.showUrl(..)` é feito do `onClick(..)` . `showUrl(..)` é transmitido a um objeto JavaScript com chave/valores:

+ `title` fornece o nome do título da modal exibida para o usuário
+ `url` é o URL que chama a variável [Reagir rota](#modal-routes) responsável pela visualização inicial da modal.

É imperativo que a `url` passado para `guestConnection.host.modal.showUrl(..)` resolve rotear na extensão do , caso contrário, nada é exibido no modal .

Revise o [menu de cabeçalho](./header-menu.md#modal) e [barra de ações](./action-bar.md#modal) documentação de como criar URLs modais.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

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

Cada rota da extensão, [não é o `index` rota](./extension-registration.md#app-routes), mapeia para um componente React que pode ser renderizado no modal da extensão.

Um modal pode ser composto por qualquer número de rotas React, de um modal simples de uma rota a um modal complexo de várias rotas.

A seguir, é ilustrada uma modal simples de uma rota. No entanto, essa visualização modal pode conter links React que chamam outras rotas ou comportamentos.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

## Feche o modal

![Botão Fechar modal da extensão Fragmento de conteúdo AEM](./assets/modal/close.png){align="center"}

Os módulos devem fornecer seu próprio controle de perto. Isso é feito invocando `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
