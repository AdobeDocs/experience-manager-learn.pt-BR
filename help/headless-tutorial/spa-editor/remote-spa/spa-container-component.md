---
title: Adicionar componentes do contêiner React editáveis a um SPA remoto
description: Saiba como adicionar componentes de contêiner editáveis a um SPA remoto que permite que AEM autores arraste e solte componentes neles.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 1%

---

# Componentes de contêiner editáveis

[Componentes fixos](./spa-fixed-component.md) oferecem alguma flexibilidade para a criação de conteúdo SPA, no entanto, essa abordagem é rígida e requer que os desenvolvedores definam a composição exata do conteúdo editável. Para suportar a criação de experiências excepcionais por autores, o Editor do SPA suporta o uso de componentes de contêiner no SPA. Os componentes do contêiner permitem aos autores arrastar e soltar componentes permitidos no contêiner e criá-los, da mesma forma que na criação tradicional do AEM Sites!

![Componentes de contêiner editáveis](./assets/spa-container-component/intro.png)

Neste capítulo, adicionamos um contêiner editável à visualização inicial, permitindo que os autores componham e façam o layout de experiências de conteúdo avançadas usando componentes React editáveis diretamente na SPA.

## Atualizar o aplicativo WKND

Para adicionar um componente de contêiner à exibição Início:

+ Importe o componente editável do React AEM `ResponsiveGrid` componente
+ Importar e registrar os Componentes Reativos Editáveis (Texto e Imagem) personalizados para uso no componente Grade Responsiva

### Usar o componente ResponsiveGrid

Para adicionar uma área editável à exibição Início:

1. Abrir e editar `react-app/src/components/Home.js`
1. Importe o `ResponsiveGrid` componente de `@adobe/aem-react-editable-components` e adicione-o à `Home` componente.
1. Defina os seguintes atributos no `<ResponsiveGrid...>` componente
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Isso instrui o `ResponsiveGrid` componente para recuperar o conteúdo do recurso AEM:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   O `itemPath` mapeia para a `responsivegrid` nó definido no `Remote SPA Page` Modelo de AEM e é criado automaticamente em novas Páginas de AEM criadas a partir do `Remote SPA Page` Modelo AEM.

   Atualizar `Home.js` para adicionar o `<ResponsiveGrid...>` componente.

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

O `Home.js` O arquivo deve ter a seguinte aparência:

![Home.js](./assets/spa-container-component/home-js.png)

## Criar componentes editáveis

Para obter o efeito total da experiência de criação flexível que os contêineres fornecem no Editor de SPA. Já criamos um componente de Título editável, mas vamos fazer mais algumas que permitem que os autores usem componentes de Texto e Imagem editáveis no componente ResponsiveGrid recém-adicionado.

Os novos componentes editáveis Texto e Reação de imagem são criados usando o padrão de definição de componente editável exportado em [componentes fixos editáveis](./spa-fixed-component.md).

### Componente de texto editável

1. Abra o SPA projeto no IDE
1. Crie um componente de reação em `src/components/editable/core/Text.js`
1. Adicione o seguinte código a `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. Crie um componente React editável em `src/components/editable/EditableText.js`
1. Adicione o seguinte código a `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

A implementação do componente de Texto editável deve ser semelhante a:

![Componente de texto editável](./assets/spa-container-component/text-js.png)

### Componente de imagem

1. Abra o SPA projeto no IDE
1. Crie um componente de reação em `src/components/editable/core/Image.js`
1. Adicione o seguinte código a `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. Crie um componente React editável em `src/components/editable/EditableImage.js`
1. Adicione o seguinte código a `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. Criar um arquivo SCSS `src/components/editable/EditableImage.scss` que fornece estilos personalizados para a `EditableImage.scss`. Esses estilos direcionam as classes CSS do componente React editável.
1. Adicione o seguinte SCSS ao `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importar `EditableImage.scss` em `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

A implementação do componente Imagem editável deve ser semelhante a:

![Componente de imagem editável](./assets/spa-container-component/image-js.png)


### Importar os componentes editáveis

O recém-criado `EditableText` e `EditableImage` Os componentes do React são referenciados no SPA e são instanciados dinamicamente com base no JSON retornado pelo AEM. Para garantir que esses componentes estejam disponíveis para o SPA, crie instruções de importação para eles em `Home.js`

1. Abra o SPA projeto no IDE
1. Abra o arquivo `src/Home.js`
1. Adicionar declarações de importação para `AEMText` e `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

O resultado deve ser semelhante a:

![Home.js](./assets/spa-container-component/home-js-imports.png)

Se essas importações _not_ adicionado, a variável `EditableText` e `EditableImage` o código não é chamado por SPA e, portanto, os componentes não são mapeados para os tipos de recursos fornecidos.

## Configuração do contêiner no AEM

AEM componentes de contêiner usam políticas para ditar seus componentes permitidos. Essa é uma configuração crítica ao usar o Editor de SPA, já que somente AEM os componentes que mapearam SPA componentes são renderizáveis pelo SPA. Verifique se apenas os componentes para os quais fornecemos implementações SPA são permitidos:

+ `EditableTitle` mapeado para `wknd-app/components/title`
+ `EditableText` mapeado para `wknd-app/components/text`
+ `EditableImage` mapeado para `wknd-app/components/image`

Para configurar o contêiner reponsivegrid do modelo da Página de SPA Remota:

1. Faça logon no AEM Author
1. Navegar para __Ferramentas > Geral > Modelos > Aplicativo WKND__
1. Editar __Página SPA do relatório__

   ![Políticas de Grade Responsivas](./assets/spa-container-component/templates-remote-spa-page.png)

1. Selecionar __Estrutura__ no alternador de modo, no canto superior direito
1. Toque para selecionar a variável __Contêiner de layout__
1. Toque no __Política__ ícone na barra pop-up

   ![Políticas de Grade Responsivas](./assets/spa-container-component/templates-policies-action.png)

1. À direita, abaixo do __Componentes permitidos__ guia , expandir __APLICATIVO WKND - CONTEÚDO__
1. Certifique-se de que somente as seguintes opções estejam selecionadas:
   + Imagem
   + Texto
   + Título

   ![Página SPA Remota](./assets/spa-container-component/templates-allowed-components.png)

1. Toque __Concluído__

## Criação do contêiner no AEM

Depois que o SPA foi atualizado para incorporar o `<ResponsiveGrid...>`, invólucros para três componentes React editáveis (`EditableTitle`, `EditableText`e `EditableImage`) e AEM é atualizado com uma política de modelo correspondente, podemos começar a criar conteúdo no componente de contêiner.

1. Faça logon no AEM Author
1. Navegar para __Sites > Aplicativo WKND__
1. Toque __Início__ e selecione __Editar__ na barra de ação superior
   + Um componente de Texto &quot;Hello World&quot; é exibido, pois ele foi adicionado automaticamente ao gerar o projeto a partir do arquétipo de Projeto AEM
1. Selecionar __Editar__ no seletor de modo na parte superior direita do Editor de páginas
1. Localize a variável __Contêiner de layout__ área editável abaixo do Título
1. Abra o __Barra lateral do Editor de páginas__ e selecione o __Exibição de componentes__
1. Arraste os seguintes componentes para o __Contêiner de layout__
   + Imagem
   + Título
1. Arraste os componentes para reorganizá-los na seguinte ordem:
   1. Título
   1. Imagem
   1. Texto
1. __Autor__ o __Título__ componente
   1. Toque no componente Título e toque no __chave inglesa__ ícone para __editar__ o componente Título
   1. Adicione o seguinte texto:
      + Título: __O verão está chegando, vamos aproveitar ao máximo!__
      + Tipo: __H1__
   1. Toque __Concluído__
1. __Autor__ o __Imagem__ componente
   1. Arraste uma imagem para a barra lateral (depois de alternar para a exibição Ativos) no componente Imagem
   1. Toque no componente de Imagem e toque no __chave inglesa__ ícone para editar
   1. Verifique a __Imagem decorativa__ caixa de seleção
   1. Toque __Concluído__
1. __Autor__ o __Texto__ componente
   1. Edite o componente de Texto ao tocar no componente de Texto e tocar no __chave inglesa__ ícone
   1. Adicione o seguinte texto:
      + _Agora, você pode obter 15% em todas as aventuras de 1 semana e 20% de desconto em todas as aventuras que tenham 2 semanas ou mais! No checkout, adicione o código da campanha SUMMERISCOMING para obter os descontos!_
   1. Toque __Concluído__

1. Seus componentes agora são criados, mas empilhados verticalmente.

   ![Componentes criados](./assets/spa-container-component/authored-components.png)

Use AEM modo Layout para permitir que ajustemos o tamanho e o layout dos componentes.

1. Mudar para __Modo de layout__ usando o seletor de modo no canto superior direito
1. __Redimensionar__ os componentes Imagem e Texto , de modo que fiquem lado a lado
   + __Imagem__ componente deve __8 colunas de largura__
   + __Texto__ componente deve __3 colunas de largura__

   ![Componentes de layout](./assets/spa-container-component/layout-components.png)

1. __Visualizar__ suas alterações no Editor de páginas AEM
1. Atualize o aplicativo WKND executado localmente em [http://localhost:3000](http://localhost:3000) para ver as alterações criadas!

   ![Componente de contêiner no SPA](./assets/spa-container-component/localhost-final.png)


## Parabéns. 

Você adicionou um componente de contêiner que permite que componentes editáveis sejam adicionados por autores ao aplicativo WKND! Agora você sabe como:

+ Usar o componente editável de reação de AEM `ResponsiveGrid` no SPA
+ Crie e registre os componentes editáveis do React (Texto e Imagem) para uso no SPA por meio do componente de contêiner
+ Configure o modelo Página de SPA Remota para permitir os componentes habilitados para SPA
+ Adicionar componentes editáveis ao componente de contêiner
+ Componentes de criação e layout no Editor SPA

## Próximas etapas

A próxima etapa usa essa mesma técnica para [adicionar um componente editável a uma rota Detalhes da Aventura](./spa-dynamic-routes.md) no SPA.
