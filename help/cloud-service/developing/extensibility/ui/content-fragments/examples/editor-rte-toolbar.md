---
title: Adicionar botão personalizado à barra de ferramentas do Editor de Rich Text (RTE)
description: Saiba como adicionar um botão personalizado à barra de ferramentas do Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Adicionar botão personalizado à barra de ferramentas do Editor de Rich Text (RTE)

Saiba como adicionar um botão personalizado à barra de ferramentas do Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

Botões personalizados podem ser adicionados à **barra de ferramentas do RTE** no Editor de Fragmentos de Conteúdo usando o ponto de extensão `rte`. Este exemplo mostra como adicionar um botão personalizado chamado _Adicionar dica_ à barra de ferramentas do RTE e modificar o conteúdo dentro do RTE.

Usando o método `getCustomButtons()` do ponto de extensão `rte`, um ou vários botões personalizados podem ser adicionados à **barra de ferramentas RTE**. Também é possível adicionar ou remover botões RTE padrão como _Copiar, Colar, Negrito e Itálico_ usando os métodos `getCoreButtons()` e `removeButtons)`, respectivamente.

Este exemplo mostra como inserir uma nota ou dica realçada usando o botão personalizado da barra de ferramentas _Adicionar Dica_. O conteúdo realçado da nota ou dica tem uma formatação especial aplicada por meio de elementos HTML e as classes CSS associadas. O conteúdo do espaço reservado e o código de HTML são inseridos usando o método de retorno de chamada `onClick()` de `getCustomButtons()`.

## Ponto de extensão

Este exemplo estende o ponto de extensão `rte` para adicionar um botão personalizado à barra de ferramentas do RTE no Editor de Fragmentos de Conteúdo.

| IU do AEM estendida | Ponto de extensão |
| ------------------------ | --------------------- | 
| [Editor de fragmento de conteúdo](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Barra de Ferramentas do Editor de Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Exemplo de extensão

O exemplo a seguir cria um botão personalizado _Adicionar dica_ na barra de ferramentas do RTE. A ação de clicar insere o texto do espaço reservado na posição atual do cursor no RTE.

O código mostra como adicionar o botão personalizado com um ícone e registrar a função de manipulador de cliques.

### Registro de extensão

`ExtensionRegistration.js`, mapeado para a rota index.html, é o ponto de entrada para a extensão AEM e define:

+ A definição do botão da barra de ferramentas do RTE na função `getCustomButtons()` com `id, tooltip and icon` atributos.
+ O manipulador de cliques do botão, na função `onClick()`.
+ A função de manipulador de cliques recebe o objeto `state` como argumento para obter o conteúdo do RTE no formato de HTML ou texto. No entanto, neste exemplo, não é usado.
+ A função de manipulador de cliques retorna uma matriz de instruções. Esta matriz tem um objeto com atributos `type` e `value`. Para inserir o conteúdo, o trecho de código HTML de `value` atributos, o atributo `type` usa o `insertContent`. Se houver um caso de uso para substituir o conteúdo, caso de uso do tipo de instrução `replaceContent`.

O valor `insertContent` é uma sequência de HTML, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. As classes CSS `cmp-contentfragment__element-tip` usadas para exibir o valor não estão definidas no widget, mas são implementadas na experiência online em que este campo Fragmento de Conteúdo é exibido.


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
          // RTE Toolbar custom button
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
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
