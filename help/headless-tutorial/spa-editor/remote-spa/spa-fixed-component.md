---
title: Adicionar componentes fixos editáveis a um SPA remoto
description: Saiba como adicionar componentes fixos editáveis a um SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
duration: 164
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 1%

---

# Componentes fixos editáveis

{{spa-editor-deprecation}}

Os componentes editáveis do React podem ser &quot;fixos&quot; ou codificados nas visualizações de SPA. Isso permite que os desenvolvedores coloquem componentes compatíveis com o Editor de SPA nas visualizações de SPA e que os usuários criem o conteúdo dos componentes no Editor de SPA do AEM.

![Componentes fixos](./assets/spa-fixed-component/intro.png)

Neste capítulo, substituímos o título do modo de exibição de Página Inicial, &quot;Aventuras Atuais&quot;, que é um texto codificado em `Home.js` com um componente de Título fixo, mas editável. Os componentes fixos garantem o posicionamento do título, mas também permitem que o texto do título seja criado e seja alterado fora do ciclo de desenvolvimento.

## Atualizar o aplicativo WKND

Para adicionar um componente __Fixo__ à exibição Início:

* Crie um componente de Título editável personalizado e registre-o no tipo de recurso do Título do projeto
* Coloque o componente de Título editável na visualização Início do SPA

### Criar um componente editável do Título do React

Na exibição Início do SPA, substitua o texto codificado `<h2>Current Adventures</h2>` por um componente Título editável personalizado. Antes que o componente de Título possa ser usado, é necessário:

1. Criar um componente personalizado do React Title
1. Decorar o componente Título personalizado usando métodos de `@adobe/aem-react-editable-components` para torná-lo editável.
1. Registre o componente de Título editável com `MapTo` para que ele possa ser usado no [componente de contêiner mais tarde](./spa-container-component.md).

Para fazer isso:

1. Abrir projeto SPA Remoto em `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` no IDE
1. Criar um componente React em `react-app/src/components/editable/core/Title.js`
1. Adicione o código a seguir a `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   Observe que este componente do React ainda não é editável usando o AEM SPA Editor. Este componente básico se tornará editável na próxima etapa.

   Leia os comentários do código para obter detalhes sobre a implementação.

1. Criar um componente React em `react-app/src/components/editable/EditableTitle.js`
1. Adicione o código a seguir a `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   Este componente do React `EditableTitle` envolve o componente do React `Title`, envolvendo-o e decorando-o para ser editável no AEM SPA Editor.

### Usar o componente EditableTitle do React

Agora que o componente EditableTitle React está registrado e disponível para uso no aplicativo React, substitua o texto de título embutido em código na exibição Início.

1. Editar `react-app/src/components/Home.js`
1. No `Home()` na parte inferior, importe `EditableTitle` e substitua o título embutido em código pelo novo componente `AEMTitle`:

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

O arquivo `Home.js` deve ser semelhante a:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## Criar o componente de Título no AEM

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND__
1. Toque em __Página inicial__ e selecione __Editar__ na barra de ações superior
1. Selecione __Editar__ no seletor de modo de edição, na parte superior direita do Editor de páginas
1. Passe o mouse sobre o texto de título padrão abaixo do logotipo WKND e acima da lista de aventuras, até que o contorno de edição azul seja exibido
1. Toque para expor a barra de ação do componente e toque na __chave inglesa__ para editar

   ![Barra de ação do componente de título](./assets/spa-fixed-component/title-action-bar.png)

1. Crie o componente de Título:
   1. Título: __Aventuras WKND__
   1. Tipo/Tamanho: __H2__

      ![Caixa de diálogo do componente de Título](./assets/spa-fixed-component/title-dialog.png)

1. Toque em __Concluído__ para salvar
1. Visualizar as alterações no AEM SPA Editor
1. Atualize o Aplicativo WKND em execução localmente em [http://localhost:3000](http://localhost:3000) e veja as alterações de título criadas refletidas imediatamente.

   ![Componente de título em SPA](./assets/spa-fixed-component/title-final.png)

## Parabéns!

Você adicionou um componente fixo e editável ao aplicativo WKND! Agora você sabe como:

* Criação de um componente fixo, mas editável, para o SPA
* Criar o componente fixo no AEM
* Ver o conteúdo criado no SPA remoto

## Próximas etapas

As próximas etapas são para [adicionar um componente de contêiner do AEM ResponsiveGrid](./spa-container-component.md) ao SPA, o que permite que o autor adicione componentes editáveis ao SPA.
