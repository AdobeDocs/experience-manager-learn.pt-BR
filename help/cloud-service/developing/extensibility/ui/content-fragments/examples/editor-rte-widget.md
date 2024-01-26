---
title: Adicionar widgets ao Rich Text Editor (RTE)
description: Saiba como adicionar widgets ao Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 475
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Adicionar widgets ao Rich Text Editor (RTE)

Saiba como adicionar widgets ao Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Para adicionar o conteúdo dinâmico no Editor de Rich Text (RTE), a variável **widgets** A funcionalidade do pode ser usada. Os widgets ajudam a integrar a interface de usuário simples ou complexa no RTE e a interface de usuário pode ser criada usando a estrutura JS de sua escolha. Elas podem ser vistas como caixas de diálogo que são abertas ao pressionar `{` chave especial no RTE.

Normalmente, os widgets são usados para inserir o conteúdo dinâmico que tem uma dependência de sistema externa ou que pode mudar com base no contexto atual.

A variável **widgets** são adicionados à **RTE** no Editor de fragmento de conteúdo usando o `rte` ponto de extensão. Usar `rte` do ponto de extensão `getWidgets()` um ou vários widgets do método são adicionados. Eles são acionados pressionando o botão `{` tecla especial para abrir a opção de menu de contexto e, em seguida, selecione o widget desejado para carregar a interface de diálogo personalizada.

Este exemplo mostra como adicionar um widget chamado _Lista de Códigos de Desconto_ para localizar, selecionar e adicionar o código de desconto específico de aventura WKND em um conteúdo RTE. Esses códigos de desconto podem ser gerenciados em um sistema externo, como o OMS (Order Management System), o PIM (Product Information Management), um aplicativo interno ou uma ação do Adobe AppBuilder.

Para simplificar as coisas, este exemplo usa o [Espectro de Reação Adobe](https://react-spectrum.adobe.com/react-spectrum/index.html) estrutura para desenvolver a interface de usuário do widget ou caixa de diálogo e o nome de aventura WKND codificado, dados de código de desconto.

## Ponto de extensão

Este exemplo se estende ao ponto de extensão `rte` para adicionar um widget ao RTE no Editor de fragmento de conteúdo.

| IU do AEM estendida | Ponto de extensão |
| ------------------------ | --------------------- | 
| [Editor de fragmento de conteúdo](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Widgets do editor de rich text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Exemplo de extensão

O exemplo a seguir cria um _Lista de Códigos de Desconto_ widget. Ao pressionar a tecla `{` especial dentro do RTE, o menu de contexto é aberto, em seguida, selecionando a variável _Lista de Códigos de Desconto_ no menu de contexto, a interface do usuário da caixa de diálogo é aberta.

Os autores de conteúdo da WKND podem encontrar, selecionar e adicionar o código de desconto específico do Adventure atual, se disponível.

### Registro de extensão

`ExtensionRegistration.js`, mapeado para a rota index.html, é o ponto de entrada para a extensão AEM e define:

+ A definição do widget em `getWidgets()` função com `id, label and url` atributos.
+ A variável `url` valor do atributo, um caminho de URL relativo (`/index.html#/discountCodes`) para carregar a interface do usuário da caixa de diálogo.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Adicionar `discountCodes` roteamento em `App.js`{#add-widgets-route}

No componente principal do React `App.js`, adicione o `discountCodes` para renderizar a interface do usuário para o caminho de URL relativo acima.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### Criar `DiscountCodes` Componente do React{#create-widget-react-component}

A interface do usuário do widget ou caixa de diálogo é criada usando o [Espectro de Reação Adobe](https://react-spectrum.adobe.com/react-spectrum/index.html) quadro regulamentar. A variável `DiscountCodes` o código do componente é como abaixo, aqui estão os principais destaques:

+ A interface é renderizada usando componentes do Espectro React, como [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [GrupoDeBotões](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Botão](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ A variável `adventureDiscountCodes` matriz tem mapeamento codificado de nome de aventura e código de desconto. Em um cenário real, esses dados podem ser recuperados da ação do AppBuilder do Adobe ou de sistemas externos, como PIM, OMS ou gateway de API crescido em casa ou baseado em provedor de nuvem.
+ A variável `guestConnection` é inicializado usando o `useEffect` [Gancho de reação](https://react.dev/reference/react/useEffect) e gerenciado como estado do componente. É usado para se comunicar com o host AEM.
+ A variável `handleDiscountCodeChange` A função obtém o código de desconto para o nome de aventura selecionado e atualiza a variável de estado.
+ A variável `addDiscountCode` função usando `guestConnection` fornece instruções de RTE para execução. Nesse caso `insertContent` instrução e trecho de código HTML do código de desconto real a ser inserido no RTE.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
