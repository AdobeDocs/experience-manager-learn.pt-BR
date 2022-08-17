---
title: Como usar AEM React Editable Components v2
description: Saiba como usar AEM React Editable Components v2 para potencializar um aplicativo React.
version: Cloud Service
feature: SPA Editor
topic: Headless
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: 18a414b847a7353eebcfad4bcc125920258948b3
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 1%

---


# Como usar AEM React Editable Components v2

AEM fornece [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), um SDK baseado em Node.js que permite a criação de componentes React, que oferecem suporte à edição de componentes no contexto usando AEM Editor de SPA.

+ [módulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Projeto Github](https://github.com/adobe/aem-react-editable-components)
+ [Documentação do Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Para obter mais detalhes e amostras de código para AEM React Editable Components v2, consulte a documentação técnica:

+ [Integração com AEM documentação](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentação do componente editável](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentação dos auxiliares](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Páginas AEM

AEM React Editable Components funcionam com aplicativos SPA Editor ou Remote SPA React. O conteúdo que preenche os componentes editáveis do React deve ser exposto por meio de AEM páginas que estendem o [Componente Página SPA](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). AEM componentes, que mapeiam para componentes React editáveis, devem implementar AEM [Estrutura do exportador de componentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - como [Componentes WCM principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR).


## Dependências

Certifique-se de que o aplicativo React esteja em execução no Node.js 14+.

O conjunto mínimo de dependências para o aplicativo React usar AEM React Editable Components v2 são: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`e  `@adobe/aem-spa-page-model-manager`.


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
> [Base de componentes WCM principal do React AEM](https://github.com/adobe/aem-react-core-wcm-components-base) e [AEM React Core WCM SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) não são compatíveis com AEM React Editable Components v2.

## Editor de SPA

Ao usar os Componentes editáveis do React do AEM com um aplicativo React baseado em SPA Editor, o AEM `ModelManager` SDK, como SDK:

1. Recupera conteúdo do AEM
1. Preenche os componentes do React Comable com AEM conteúdo

Vincule o aplicativo React a um ModelManager inicializado e renderize o aplicativo React. O aplicativo React deve conter uma instância do `<Page>` componente exportado de `@adobe/aem-react-editable-components`. O `<Page>` tem lógica para criar dinamicamente componentes React com base no `.model.json` fornecido pela AEM.

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

O `<Page>` é passado como a representação da página AEM como JSON, por meio do `pageModel` do `ModelManager`. O `<Page>` O componente cria dinamicamente componentes React para objetos na `pageModel` ao corresponder a `resourceType` com um componente React que se registra no tipo de recurso por meio de `MapTo(..)`.

## Componentes editáveis

O `<Page>` é transmitido à representação da página de AEM como JSON, por meio do `ModelManager`. O `<Page>` em seguida, o componente cria dinamicamente componentes React para cada objeto no JSON ao corresponder ao objeto JS `resourceType` com um componente React que se registra no tipo de recurso por meio do componente `MapTo(..)` invocação . Por exemplo, o seguinte seria usado para instanciar uma instância

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

O JSON fornecido pela AEM acima pode ser usado para instanciar e preencher dinamicamente um componente React editável.

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
 * If this component will be embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Como incorporar componentes

Os componentes editáveis podem ser reutilizados e incorporados uns nos outros. Há duas considerações principais ao incorporar um componente editável em outro:

1. O conteúdo JSON do AEM para o componente de incorporação deve conter o conteúdo para atender aos componentes incorporados. Isso é feito criando uma caixa de diálogo para o componente de AEM que coleta os dados necessários.
1. A instância &quot;não editável&quot; do componente React deve ser incorporada, em vez da instância &quot;editável&quot; que está envolvida com `<EditableComponent>`. O motivo é, se o componente incorporado tiver o `<EditableComponent>` wrapper, o Editor de SPA tenta vestir o componente interno com o cromo de edição (caixa azul de flutuação), em vez do componente de incorporação externo.

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

O JSON fornecido pela AEM acima pode ser usado para instanciar e preencher dinamicamente um componente React editável que incorpora outro componente React.


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



