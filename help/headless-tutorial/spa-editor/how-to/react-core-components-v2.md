---
title: Como usar os componentes editáveis do AEM React v2
description: Saiba como usar o AEM React Editable Components v2 para potencializar um aplicativo React.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 244
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 0%

---

# Como usar os componentes editáveis do AEM React v2

{{edge-delivery-services}}

O AEM fornece [Componentes editáveis do AEM React v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), um SDK baseado em Node.js que permite a criação de componentes do React, que oferecem suporte à edição de componentes no contexto usando o Editor de SPA AEM.

+ [módulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Projeto do GitHub](https://github.com/adobe/aem-react-editable-components)
+ [Documentação do Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Para obter mais detalhes e amostras de código para os Componentes editáveis AEM React v2, consulte a documentação técnica:

+ [Integração com a documentação do AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentação de componente editável](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentação de auxiliares](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Páginas AEM

Componentes editáveis AEM React funcionam com o Editor de SPA ou com os aplicativos SPA React remotos. O conteúdo que preenche os componentes editáveis do React deve ser exposto por meio de páginas AEM que estendem o [Componente da página SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). Componentes do AEM, que mapeiam para componentes editáveis do React, devem implementar o AEM [Estrutura do exportador de componentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - como [Componentes WCM principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR).


## Dependências

Certifique-se de que o aplicativo React está em execução no Node.js 14+.

O conjunto mínimo de dependências para o aplicativo React usar os componentes editáveis AEM React v2 é: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`, e  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [Base de componentes WCM do núcleo de reação do AEM](https://github.com/adobe/aem-react-core-wcm-components-base) e [Componentes WCM do núcleo de reação do AEM SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) não são compatíveis com os Componentes editáveis AEM React v2.

## Editor de SPA

Ao usar os componentes editáveis do AEM React com um aplicativo React baseado em SPA, o AEM `ModelManager` SDK, como o SDK:

1. Recupera conteúdo do AEM
1. Preenche os componentes comestíveis do React com conteúdo AEM

Vincule o aplicativo React com um ModelManager inicializado e renderize o aplicativo React. O aplicativo React deve conter uma instância do `<Page>` componente exportado de `@adobe/aem-react-editable-components`. A variável `<Page>` componente tem lógica para criar dinamicamente componentes do React com base na `.model.json` fornecido pelo AEM.

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

A variável `<Page>` é transmitido como a representação da página AEM como JSON, por meio da `pageModel` fornecido pelo `ModelManager`. A variável `<Page>` componente cria dinamicamente componentes do React para objetos na `pageModel` ao corresponder ao `resourceType` com um componente React que se registra no tipo de recurso por meio de `MapTo(..)`.

## Componentes editáveis

A variável `<Page>` é transmitida a representação da página AEM como JSON, por meio da `ModelManager`. A variável `<Page>` em seguida, cria dinamicamente componentes React para cada objeto no JSON, correspondendo ao `resourceType` com um componente React que se registra no tipo de recurso por meio da função `MapTo(..)` invocação. Por exemplo, seria usado o seguinte para instanciar uma instância

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

O JSON acima fornecido pelo AEM pode ser usado para instanciar e preencher dinamicamente um componente editável do React.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Como incorporar componentes

Os componentes editáveis podem ser reutilizados e incorporados uns aos outros. Há duas considerações principais ao incorporar um componente editável em outro:

1. O conteúdo JSON do AEM para o componente de incorporação deve conter o conteúdo para atender aos componentes incorporados. Isso é feito criando uma caixa de diálogo para o componente AEM que coleta os dados necessários.
1. A instância &quot;não editável&quot; do componente React deve ser incorporada, em vez da instância &quot;editável&quot; que está encapsulada com `<EditableComponent>`. O motivo é, se o componente incorporado tiver o `<EditableComponent>` empacotador, o Editor de SPA tenta vestir o componente interno com o chrome de edição (caixa de flutuação azul), em vez do componente de incorporação externo.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

O JSON acima fornecido pelo AEM pode ser usado para instanciar e preencher dinamicamente um componente editável do React que incorpora outro componente do React.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```
