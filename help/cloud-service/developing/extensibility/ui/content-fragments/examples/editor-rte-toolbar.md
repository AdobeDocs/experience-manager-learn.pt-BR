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
duration: 397
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Adicionar botão personalizado à barra de ferramentas do Editor de Rich Text (RTE)

Saiba como adicionar um botão personalizado à barra de ferramentas do Editor de Rich Text (RTE) no Editor de fragmento de conteúdo do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

Botões personalizados podem ser adicionados à **Barra de ferramentas do RTE** no Editor de fragmento de conteúdo usando o `rte` ponto de extensão. Este exemplo mostra como adicionar um botão personalizado chamado _Adicionar Dica_ à barra de ferramentas do RTE e modifique o conteúdo no RTE.

Usar `rte` do ponto de extensão `getCustomButtons()` método um ou vários botões personalizados podem ser adicionados à **Barra de ferramentas do RTE**. Também é possível adicionar ou remover botões RTE padrão, como _Copiar, Colar, Negrito e Itálico_ usar `getCoreButtons()` e `removeButtons)` métodos, respectivamente.

Este exemplo mostra como inserir uma nota ou dica realçada usando um _Adicionar Dica_ botão da barra de ferramentas. O conteúdo realçado da nota ou dica tem uma formatação especial aplicada por meio de elementos HTML e as classes CSS associadas. O conteúdo do espaço reservado e o código HTML são inseridos usando o `onClick()` método de retorno de chamada do `getCustomButtons()`.

## Ponto de extensão

Este exemplo se estende ao ponto de extensão `rte` para adicionar um botão personalizado à barra de ferramentas do RTE no Editor de fragmento de conteúdo.

| IU do AEM estendida | Ponto de extensão |
| ------------------------ | --------------------- | 
| [Editor de fragmento de conteúdo](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Barra de Ferramentas do Editor de Rich Text](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Exemplo de extensão

O exemplo a seguir cria um _Adicionar Dica_ na barra de ferramentas do RTE. A ação de clicar insere o texto do espaço reservado na posição atual do cursor no RTE.

O código mostra como adicionar o botão personalizado com um ícone e registrar a função de manipulador de cliques.

### Registro de extensão

`ExtensionRegistration.js`, mapeado para a rota index.html, é o ponto de entrada para a extensão AEM e define:

+ A definição do botão da barra de ferramentas do RTE em `getCustomButtons()` função com `id, tooltip and icon` atributos.
+ O manipulador de cliques do botão, na caixa `onClick()` função.
+ A função de manipulador de cliques recebe a variável `state` como um argumento para obter o conteúdo do RTE no formato HTML ou texto. No entanto, neste exemplo, não é usado.
+ A função de manipulador de cliques retorna uma matriz de instruções. Esta matriz tem um objeto com `type` e `value` atributos. Para inserir o conteúdo, a variável `value` atributos trecho de código HTML, a variável `type` o atributo usa o `insertContent`. Se houver um caso de uso para substituir o conteúdo, caso de uso o `replaceContent` tipo de instrução.

A variável `insertContent` o valor é uma string de HTML, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. As classes CSS `cmp-contentfragment__element-tip` Os usados para exibir o valor não são definidos no widget, mas implementados na experiência da Web em que esse campo Fragmento de conteúdo é exibido.


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

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
