---
title: Adicionar componentes fixos editáveis a um SPA remoto
description: Saiba como adicionar componentes fixos editáveis a um SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# Componentes fixos editáveis

Os componentes editáveis do React podem ser &quot;fixos&quot; ou codificados nas visualizações do SPA. Isso permite que os desenvolvedores coloquem componentes compatíveis com o Editor de SPA nas visualizações de SPA e que os usuários criem o conteúdo dos componentes no Editor de AEM SPA.

![Componentes fixos](./assets/spa-fixed-component/intro.png)

Neste capítulo, substituímos o título da visualização inicial, &quot;Aventuras atuais&quot;, que é um texto inserido no código `Home.js` com um componente de Título fixo, mas editável. Os componentes fixos garantem o posicionamento do título, mas também permitem que o texto do título seja criado e seja alterado fora do ciclo de desenvolvimento.

## Atualizar o aplicativo WKND

Para adicionar um __Fixo__ componente para a visualização Início:

+ Crie um componente de Título editável personalizado e registre-o no tipo de recurso do Título do projeto
+ Inserir o componente de Título editável na visualização inicial do SPA

### Criar um componente editável do Título do React

Na exibição Início do SPA, substitua o texto embutido em código `<h2>Current Adventures</h2>` com um componente de Título editável personalizado. Antes que o componente de Título possa ser usado, é necessário:

1. Criar um componente personalizado do React Title
1. Decorar o componente de Título personalizado usando métodos de `@adobe/aem-react-editable-components` para torná-la editável.
1. Registrar o componente de Título editável com `MapTo` para que possa ser usado em [componente do contêiner mais tarde](./spa-container-component.md).

Para fazer isso:

1. Abrir projeto do SPA remoto em `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` no IDE
1. Criar um componente do React em `react-app/src/components/editable/core/Title.js`
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

   Observe que este componente do React ainda não é editável usando o Editor SPA AEM. Este componente básico se tornará editável na próxima etapa.

   Leia os comentários do código para obter detalhes sobre a implementação.

1. Criar um componente do React em `react-app/src/components/editable/EditableTitle.js`
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

   Este `EditableTitle` O componente React envolve o `Title` Componente do React, envolvido e decorado para ser editável no Editor de SPA AEM.

### Usar o componente EditableTitle do React

Agora que o componente EditableTitle React está registrado e disponível para uso no aplicativo React, substitua o texto de título embutido em código na exibição Início.

1. Editar `react-app/src/components/Home.js`
1. No `Home()` na parte inferior, importe `EditableTitle` e substitua o título codificado pelo novo `AEMTitle` componente:

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

A variável `Home.js` O arquivo deve ter a seguinte aparência:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## Criar o componente de Título no AEM

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND__
1. Toque __Início__ e selecione __Editar__ na barra de ação superior
1. Selecionar __Editar__ no seletor de modo de edição, na parte superior direita do Editor de páginas
1. Passe o mouse sobre o texto de título padrão abaixo do logotipo WKND e acima da lista de aventuras, até que o contorno de edição azul seja exibido
1. Toque para expor a barra de ação do componente e toque em __chave inglesa__  para editar

   ![Barra de ação do componente do título](./assets/spa-fixed-component/title-action-bar.png)

1. Crie o componente de Título:
   + Título: __Aventuras WKND__
   + Tipo/tamanho: __H2__

      ![Caixa de diálogo do componente de Título](./assets/spa-fixed-component/title-dialog.png)

1. Toque __Concluído__ para salvar
1. Visualizar as alterações no Editor SPA do AEM
1. Atualizar o aplicativo WKND em execução localmente em [http://localhost:3000](http://localhost:3000) e veja as alterações de título criadas imediatamente refletidas.

   ![Componente de título no SPA](./assets/spa-fixed-component/title-final.png)

## Parabéns!

Você adicionou um componente fixo e editável ao aplicativo WKND! Agora você sabe como:

+ Criação de um componente fixo, mas editável, para o SPA
+ Criar o componente fixo no AEM
+ Ver o conteúdo criado no SPA remoto

## Próximas etapas

As próximas etapas são [adicionar um componente de contêiner AEM ResponsiveGrid](./spa-container-component.md) ao SPA que permite que o autor adicione componentes editáveis ao SPA!
