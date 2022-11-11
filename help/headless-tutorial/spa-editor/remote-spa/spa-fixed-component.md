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

Os componentes React editáveis podem ser &quot;fixos&quot; ou codificados nas visualizações de SPA. Isso permite que os desenvolvedores coloquem SPA componentes compatíveis com o Editor nas visualizações de SPA e que os usuários criem o conteúdo dos componentes AEM Editor SPA.

![Componentes fixos](./assets/spa-fixed-component/intro.png)

Neste capítulo, substituímos o título da exibição Início, &quot;Aventuras Atuais&quot;, que é um texto embutido em código fixo em `Home.js` com um componente de Título fixo, mas editável. Os componentes fixos garantem o posicionamento do título, mas também permitem a criação do texto do título e a alteração fora do ciclo de desenvolvimento.

## Atualizar o aplicativo WKND

Para adicionar uma __Fixo__ para a exibição Início:

+ Crie um componente de Título editável personalizado e registre-o no tipo de recurso de Título do projeto
+ Coloque o componente Título editável na exibição Início do SPA

### Criar um componente de Título de reação editável

Na exibição Início do SPA, substitua o texto embutido `<h2>Current Adventures</h2>` com um componente de Título editável personalizado. Antes do componente Título poder ser usado, é necessário:

1. Criar um componente personalizado de Reação de título
1. Decorre o componente de Título personalizado usando métodos de `@adobe/aem-react-editable-components` para torná-lo editável.
1. Registre o componente de Título editável com `MapTo` para que possa ser usado em [componente de contêiner mais tarde](./spa-container-component.md).

Para fazer isso:

1. Abrir projeto de SPA remoto em `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` no IDE
1. Crie um componente de reação em `react-app/src/components/editable/core/Title.js`
1. Adicione o seguinte código a `Title.js`.

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

   Observe que esse componente React ainda não é editável usando AEM Editor SPA. Esse componente básico será editável na próxima etapa.

   Leia os comentários do código para obter os detalhes de implementação.

1. Crie um componente de reação em `react-app/src/components/editable/EditableTitle.js`
1. Adicione o seguinte código a `EditableTitle.js`.

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

   Essa `EditableTitle` O componente de reação envolve o `Title` React o componente, envolvendo e decorando para ser editável AEM Editor SPA.

### Usar o componente Título editável do React

Agora que o componente React do Título Editável está registrado e disponível para uso no aplicativo React, substitua o texto do título codificado na exibição Início.

1. Editar `react-app/src/components/Home.js`
1. No `Home()` na parte inferior, importe `EditableTitle` e substitua o título embutido pelo novo `AEMTitle` componente:

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

O `Home.js` O arquivo deve ter a seguinte aparência:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## Crie o componente de Título no AEM

1. Faça logon no AEM Author
1. Navegar para __Sites > Aplicativo WKND__
1. Toque __Início__ e selecione __Editar__ na barra de ação superior
1. Selecionar __Editar__ no seletor de modo de edição na parte superior direita do Editor de páginas
1. Passe o mouse sobre o texto de título padrão abaixo do logotipo WKND e acima da lista de aventuras, até que o contorno de edição azul seja exibido
1. Toque em para expor a barra de ação do componente e toque em __chave inglesa__  para editar

   ![Barra de ação do componente de título](./assets/spa-fixed-component/title-action-bar.png)

1. Crie o componente Título :
   + Título: __Aventuras WKND__
   + Tipo/tamanho: __H2__

      ![Caixa de diálogo do componente de título](./assets/spa-fixed-component/title-dialog.png)

1. Toque __Concluído__ para salvar
1. Visualizar as alterações no AEM Editor SPA
1. Atualize o aplicativo WKND executado localmente em [http://localhost:3000](http://localhost:3000) e ver as alterações no título de criação imediatamente refletidas.

   ![Componente de título em SPA](./assets/spa-fixed-component/title-final.png)

## Parabéns. 

Você adicionou um componente fixo e editável ao aplicativo WKND! Agora você sabe como:

+ Criado um componente fixo, mas editável, para o SPA
+ Crie o componente fixo no AEM
+ Ver o conteúdo criado no SPA remoto

## Próximas etapas

As próximas etapas são para [adicionar um componente do contêiner ResponsiveGrid de AEM](./spa-container-component.md) à SPA que permite que o autor adicione e edite componentes à SPA!
