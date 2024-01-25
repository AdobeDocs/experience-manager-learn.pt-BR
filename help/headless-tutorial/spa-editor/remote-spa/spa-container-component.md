---
title: Adicionar componentes editáveis do contêiner React a um SPA remoto
description: Saiba como adicionar componentes editáveis do contêiner a um SPA remoto que permite que os autores do AEM arrastem e soltem componentes neles.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 399
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 1%

---

# Componentes de contêiner editáveis

[Componentes fixos](./spa-fixed-component.md) oferecem alguma flexibilidade para a criação de conteúdo SPA, no entanto, essa abordagem é rígida e requer que os desenvolvedores definam a composição exata do conteúdo editável. Para auxiliar na criação de experiências excepcionais pelos autores, o Editor de SPA apoia o uso de componentes de contêiner no SPA. Os componentes do contêiner permitem que os autores arrastem e soltem componentes permitidos no contêiner e os criem, da mesma forma que fazem na criação tradicional do AEM Sites.

![Componentes de contêiner editáveis](./assets/spa-container-component/intro.png)

Neste capítulo, adicionamos um contêiner editável à visualização inicial, permitindo que os autores componham e façam o layout de experiências ricas de conteúdo usando componentes editáveis do React diretamente no SPA.

## Atualizar o aplicativo WKND

Para adicionar um componente de contêiner à exibição Início:

+ Importar do componente editável AEM React `ResponsiveGrid` componente
+ Importar e registrar Componentes editáveis personalizados do React (Texto e Imagem) para uso no componente ResponsiveGrid

### Uso do componente ResponsiveGrid

Para adicionar uma área editável à exibição Início:

1. Abrir e editar `react-app/src/components/Home.js`
1. Importe o `ResponsiveGrid` componente de `@adobe/aem-react-editable-components` e adicione-o à `Home` componente.
1. Defina os seguintes atributos no `<ResponsiveGrid...>` componente
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Isso instrui o `ResponsiveGrid` para recuperar seu conteúdo do recurso AEM:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   A variável `itemPath` mapeia para o `responsivegrid` nó definido na variável `Remote SPA Page` Modelo AEM e é criado automaticamente em novas Páginas AEM criadas a partir do `Remote SPA Page` Modelo AEM.

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

A variável `Home.js` O arquivo deve ter a seguinte aparência:

![Home.js](./assets/spa-container-component/home-js.png)

## Criar componentes editáveis

Para obter o efeito total dos contêineres flexíveis de experiência de criação fornecidos no Editor de SPA. Já criamos um componente de Título editável, mas vamos fazer mais algumas coisas que permitem aos autores usar componentes de Texto e Imagem editáveis no componente ResponsiveGrid recém-adicionado.

Os novos componentes editáveis Texto e Imagem React são criados usando o padrão de definição do componente editável exportado em [componentes fixos editáveis](./spa-fixed-component.md).

### Componente de texto editável

1. Abra o projeto SPA no IDE
1. Criar um componente do React em `src/components/editable/core/Text.js`
1. Adicione o código a seguir a `Text.js`

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

1. Criar um componente editável do React em `src/components/editable/EditableText.js`
1. Adicione o código a seguir a `EditableText.js`

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

1. Abra o projeto SPA no IDE
1. Criar um componente do React em `src/components/editable/core/Image.js`
1. Adicione o código a seguir a `Image.js`

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

1. Criar um componente editável do React em `src/components/editable/EditableImage.js`
1. Adicione o código a seguir a `EditableImage.js`

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


1. Criar um arquivo SCSS `src/components/editable/EditableImage.scss` que fornece estilos personalizados para o `EditableImage.scss`. Esses estilos têm como alvo as classes CSS do componente React editável.
1. Adicione o seguinte SCSS a `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importar `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

A implementação do componente de Imagem editável deve ser semelhante a:

![Componente de imagem editável](./assets/spa-container-component/image-js.png)


### Importar os componentes editáveis

O recém-criado `EditableText` e `EditableImage` Os componentes do React são referenciados no SPA e são instanciados dinamicamente com base no JSON retornado pelo AEM. Para garantir que esses componentes estejam disponíveis para o SPA, crie instruções de importação para eles no `Home.js`

1. Abra o projeto SPA no IDE
1. Abra o arquivo `src/Home.js`
1. Adicionar instruções de importação para `AEMText` e `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

O resultado deve ser semelhante a:

![Home.js](./assets/spa-container-component/home-js-imports.png)

Se essas importações forem _não_ adicionada, a variável `EditableText` e `EditableImage` o código não é chamado pelo SPA e, portanto, os componentes não são mapeados para os tipos de recursos fornecidos.

## Configuração do contêiner no AEM

Componentes de contêiner AEM usam políticas para ditar seus componentes permitidos. Essa é uma configuração crítica ao usar o Editor de SPA, já que somente os componentes AEM que têm equivalentes de componentes SPA mapeados podem ser renderizados pelo SPA. Verifique se somente os componentes para os quais fornecemos implementações de SPA são permitidos:

+ `EditableTitle` mapeado para `wknd-app/components/title`
+ `EditableText` mapeado para `wknd-app/components/text`
+ `EditableImage` mapeado para `wknd-app/components/image`

Para configurar o contêiner reponsivegrid do modelo da Página do SPA Remota:

1. Faça logon no AEM Author
1. Navegue até __Ferramentas > Geral > Modelos > Aplicativo WKND__
1. Editar __Página Denunciar SPA__

   ![Políticas de Grade Responsivas](./assets/spa-container-component/templates-remote-spa-page.png)

1. Selecionar __Estrutura__ no alternador de modo no canto superior direito
1. Toque para selecionar __Contêiner de layout__
1. Toque no __Política__ ícone na barra pop-up

   ![Políticas de Grade Responsivas](./assets/spa-container-component/templates-policies-action.png)

1. À direita, sob o __Componentes permitidos__ guia, expandir __APLICATIVO WKND - CONTEÚDO__
1. Certifique-se de que apenas os seguintes sejam selecionados:
   + Imagem
   + Texto
   + Título

   ![Página remota do SPA](./assets/spa-container-component/templates-allowed-components.png)

1. Toque __Concluído__

## Criação do contêiner no AEM

Depois que o SPA foi atualizado para incorporar o `<ResponsiveGrid...>`, invólucros para três componentes editáveis do React (`EditableTitle`, `EditableText`, e `EditableImage`) e o AEM for atualizado com uma política de modelo correspondente, podemos começar a criar conteúdo no componente de contêiner.

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND__
1. Toque __Início__ e selecione __Editar__ na barra de ação superior
   + Um componente Texto &quot;Olá, mundo&quot; é exibido, pois ele é adicionado automaticamente ao gerar o projeto a partir do arquétipo do projeto AEM
1. Selecionar __Editar__ no seletor de modo na parte superior direita do Editor de páginas
1. Localize o __Contêiner de layout__ área editável abaixo do Título
1. Abra o __Barra lateral do Editor de páginas__ e selecione a variável __Visualização Componentes__
1. Arraste os seguintes componentes para a __Contêiner de layout__
   + Imagem
   + Título
1. Arraste os componentes para reordená-los na seguinte ordem:
   1. Título
   1. Imagem
   1. Texto
1. __Autor__ o __Título__ componente
   1. Toque no componente Título e toque no __chave inglesa__ ícone para __editar__ o componente de Título
   1. Adicione o seguinte texto:
      + Título: __O verão está chegando, vamos aproveitar ao máximo!__
      + Tipo: __H1__
   1. Toque __Concluído__
1. __Autor__ o __Imagem__ componente
   1. Arraste uma imagem para o a partir da barra Lateral (após alternar para a exibição Ativos) no componente Imagem
   1. Toque no componente de Imagem e em __chave inglesa__ ícone para editar
   1. Verifique a __A imagem é decorativa__ caixa de seleção
   1. Toque __Concluído__
1. __Autor__ o __Texto__ componente
   1. Edite o componente de Texto tocando no componente de Texto e tocando no __chave inglesa__ ícone
   1. Adicione o seguinte texto:
      + _Agora, você pode obter 15% em todas as aventuras de 1 semana e 20% de desconto em todas as aventuras que são de 2 semanas ou mais! No checkout, adicione o código da campanha SUMMERISCOMING para obter seus descontos!_
   1. Toque __Concluído__

1. Seus componentes agora foram criados, mas são empilhados verticalmente.

   ![Componentes criados](./assets/spa-container-component/authored-components.png)

Use o Modo de layout AEM para permitir que ajustemos o tamanho e o layout dos componentes.

1. Alternar para __Modo de layout__ usando o seletor de modo no canto superior direito
1. __Redimensionar__ os componentes Imagem e Texto, de forma que fiquem lado a lado
   + __Imagem__ deve ser __8 colunas de largura__
   + __Texto__ deve ser __3 colunas de largura__

   ![Componentes de layout](./assets/spa-container-component/layout-components.png)

1. __Visualizar__ suas alterações no Editor de página AEM
1. Atualizar o aplicativo WKND em execução localmente em [http://localhost:3000](http://localhost:3000) para ver as alterações criadas!

   ![Componente de contêiner no SPA](./assets/spa-container-component/localhost-final.png)


## Parabéns.

Você adicionou um componente de contêiner que permite que os componentes editáveis sejam adicionados pelos autores ao aplicativo WKND! Agora você sabe como:

+ Usar o componente editável AEM React `ResponsiveGrid` componente no SPA
+ Criar e registrar componentes editáveis do React (Texto e Imagem) para uso no SPA por meio do componente de container
+ Configurar o modelo da página SPA remota para permitir os componentes habilitados para SPA
+ Adicionar componentes editáveis ao componente do contêiner
+ Componentes do autor e do layout no Editor de SPA

## Próximas etapas

A próxima etapa utiliza essa mesma técnica para [adicionar um componente editável a uma rota de Detalhes de aventura](./spa-dynamic-routes.md) no SPA.
