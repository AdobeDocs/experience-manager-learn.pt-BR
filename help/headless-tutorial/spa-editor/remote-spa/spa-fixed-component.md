---
title: Adicionar componentes fixos editáveis a um SPA remoto
description: Saiba como adicionar componentes fixos editáveis a um SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---

# Componentes fixos editáveis

Os componentes React editáveis podem ser &quot;fixos&quot; ou codificados nas visualizações de SPA. Isso permite que os desenvolvedores coloquem SPA componentes compatíveis com o Editor nas visualizações de SPA e que os usuários criem o conteúdo dos componentes AEM Editor SPA.

![Componentes fixos](./assets/spa-fixed-component/intro.png)

Neste capítulo, substituímos o título da exibição Início, &quot;Aventuras Atuais&quot;, que é um texto embutido em código fixo em `Home.js` com um componente de Título fixo, mas editável. Os componentes fixos garantem o posicionamento do título, mas também permitem a criação do texto do título e a alteração fora do ciclo de desenvolvimento.

## Atualizar o aplicativo WKND

Para adicionar uma __Fixo__ para a exibição Início:

+ Importe o componente Título do componente principal de reação de AEM e registre-o no tipo de recurso Título do projeto
+ Coloque o componente Título editável na exibição Início do SPA

### Importar no componente Título do Componente principal de reação do AEM

Na exibição Início do SPA, substitua o texto embutido `<h2>Current Adventures</h2>` com o componente Título dos componentes principais do AEM React. Antes do componente Título poder ser usado, é necessário:

1. Importar o componente de Título de `@adobe/aem-core-components-react-base`
1. Registre-o usando `withMappable` para que os desenvolvedores possam colocá-lo no SPA
1. Além disso, registre-se com `MapTo` para que possa ser usado em [componente de contêiner mais tarde](./spa-container-component.md).

Para fazer isso:

1. Abrir projeto de SPA remoto em `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` no IDE
1. Crie um componente de reação em `react-app/src/components/aem/AEMTitle.js`
1. Adicione o seguinte código a `AEMTitle.js`.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

Leia os comentários do código para obter os detalhes de implementação.

O `AEMTitle.js` O arquivo deve ter a seguinte aparência:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Use o componente React AEMTitle

Agora que o componente Título do Componente principal do React do AEM está registrado e disponível para uso no aplicativo React, substitua o texto do título codificado na exibição Início.

1. Editar `react-app/src/Home.js`
1. No `Home()` na parte inferior, substitua o título embutido pelo novo `AEMTitle` componente:

   ```
   <h2>Current Adventures</h2>
   ```

   com

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Atualizar `Home.js` com o seguinte código:

   ```
   ...
   import { AEMTitle } from './aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

O `Home.js` O arquivo deve ter a seguinte aparência:

![Home.js](./assets/spa-fixed-component/home-js.png)

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

+ Importar e reutilizar um Componente principal de reação AEM no SPA
+ Adicionar um componente fixo, mas editável, ao SPA
+ Crie o componente fixo no AEM
+ Ver o conteúdo criado no SPA remoto

## Próximas etapas

As próximas etapas são para [adicionar um componente do contêiner ResponsiveGrid de AEM](./spa-container-component.md) à SPA que permite que o autor adicione e edite componentes à SPA!
