---
title: Adicionar selos ao Rich Text Editor (RTE)
description: Saiba como adicionar selos ao Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# Adicionar selos ao Rich Text Editor (RTE)

Saiba como adicionar selos ao Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[Selo do Editor de Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) são extensões que tornam o texto no Editor de Rich Text (RTE) não editável. Isso significa que um selo declarado como tal só pode ser completamente removido e não pode ser parcialmente editado. Esses selos também oferecem suporte a cores especiais no RTE, indicando claramente aos autores de conteúdo que o texto é um selo e, portanto, não é editável. Além disso, fornecem dicas visuais sobre o significado do texto do selo.

O caso de uso mais comum para selos RTE é usá-los em conjunto com [widgets RTE](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). Isso permite que o conteúdo inserido no RTE pelo dispositivo RTE seja não editável.

Normalmente, os emblemas em associação com os widgets são usados para adicionar o conteúdo dinâmico que tem uma dependência de sistema externa, mas _os autores de conteúdo não podem modificar_ o conteúdo dinâmico inserido para manter a integridade. Elas só podem ser removidas como um item inteiro.

As **medalhas** são adicionadas ao **RTE** no Editor de Fragmento de Conteúdo usando o ponto de extensão `rte`. Usando o método `getBadges()` do ponto de extensão `rte`, uma ou várias medalhas são adicionadas.

Este exemplo mostra como adicionar um widget chamado _Atendimento ao cliente das reservas de grandes grupos_ para localizar, selecionar e adicionar os detalhes do atendimento ao cliente específicos da aventura WKND, como **Nome do representante** e **Número de telefone**, em um conteúdo RTE. Usando a funcionalidade de medalhas, o **Número de Telefone** tornou-se **não editável**, mas os autores de conteúdo WKND podem editar o Nome de Representante.

Além disso, o **Número de Telefone** tem um estilo diferente (azul), o que é um caso de uso extra da funcionalidade de selos.

Para simplificar as coisas, este exemplo usa a estrutura [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) para desenvolver a interface do usuário do widget ou da caixa de diálogo e números de telefone do Serviço de Atendimento ao Cliente da WKND embutidos em código. Para controlar o aspecto de não edição e de estilo diferente do conteúdo, o caractere `#` é usado no atributo `prefix` e `suffix` da definição dos emblemas.

## Pontos de extensão

Este exemplo se estende ao ponto de extensão `rte` para adicionar um selo ao RTE no Editor de Fragmento de Conteúdo.

| IU do AEM estendida | Pontos de extensão |
| ------------------------ | --------------------- | 
| [Editor de fragmento de conteúdo](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Selos do Editor de Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) e [Widgets do Editor de Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Exemplo de extensão

O exemplo a seguir cria um widget de _Serviço de Atendimento ao Cliente com Reservas de Grupos Grandes_. Ao pressionar a tecla `{` no RTE, o menu de contexto dos widgets do RTE é aberto. Ao selecionar a opção _Serviço de Atendimento ao Cliente de Reservas de Grupos Grandes_ no menu de contexto, o modal personalizado é aberto.

Depois que o número de atendimento ao cliente desejado é adicionado a partir do modal, os selos tornam o _Número de Telefone não editável_ e o estimula na cor azul.

### Registro de extensão

`ExtensionRegistration.js`, mapeado para a rota `index.html`, é o ponto de entrada para a extensão AEM e define:

+ A definição do selo é definida em `getBadges()` usando os atributos de configuração `id`, `prefix`, `suffix`, `backgroundColor` e `textColor`.
+ Neste exemplo, o caractere `#` é usado para definir os limites deste selo, o que significa que qualquer cadeia de caracteres no RTE rodeada por `#` é tratada como uma instância deste selo.

Além disso, consulte os principais detalhes do widget RTE:

+ A definição do widget na função `getWidgets()` com os atributos `id`, `label` e `url`.
+ O valor do atributo `url`, um caminho de URL relativo (`/index.html#/largeBookingsCustomerService`) para carregar o modal.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Adicionar rota `largeBookingsCustomerService` em `App.js`{#add-widgets-route}

No componente principal do React `App.js`, adicione a rota `largeBookingsCustomerService` para renderizar a interface para o caminho de URL relativo acima.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### Criar o componente React `LargeBookingsCustomerService`{#create-widget-react-component}

A interface de widget ou caixa de diálogo é criada usando a estrutura [Espectro de Reação Adobe](https://react-spectrum.adobe.com/react-spectrum/index.html).

O código do componente React ao adicionar os detalhes de atendimento ao cliente, cercar a variável de número de telefone com o caractere `#` de selos registrados para convertê-la em selos, como `#${phoneNumber}#`, tornando-a não editável.

Estes são os principais destaques do código `LargeBookingsCustomerService`:

+ A interface é renderizada usando componentes do Espectro React, como [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ A matriz `largeGroupCustomerServiceList` tem mapeamento codificado de nome de representante e número de telefone. Em um cenário real, esses dados podem ser recuperados da ação do Adobe AppBuilder ou de sistemas externos, ou de um gateway de API crescido em casa ou baseado em provedor de nuvem.
+ O `guestConnection` é inicializado usando o `useEffect` [Gancho de reação](https://react.dev/reference/react/useEffect) e gerenciado como estado de componente. É usado para se comunicar com o host AEM.
+ A função `handleCustomerServiceChange` obtém o nome representativo e o número de telefone e atualiza as variáveis de estado do componente.
+ A função `addCustomerServiceDetails` que usa o objeto `guestConnection` fornece instruções de RTE para execução. Neste caso, instrução `insertContent` e trecho de código HTML.
+ Para tornar o **número de telefone não editável** usando medalhas, o caractere especial `#` é adicionado antes e depois da variável `phoneNumber`, como `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
