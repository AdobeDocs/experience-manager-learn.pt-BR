---
title: Mapear componentes SPA para AEM componentes | Introdução ao editor de SPA AEM e reação
description: Saiba como mapear os componentes React para o Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas para SPA componentes no AEM SPA Editor, de forma semelhante à criação tradicional AEM.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '2259'
ht-degree: 1%

---


# Mapear componentes SPA para AEM componentes {#map-components}

Saiba como mapear os componentes React para o Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas para SPA componentes no AEM SPA Editor, de forma semelhante à criação tradicional AEM.

Este capítulo aprofunda a API do modelo JSON AEM e mostra como o conteúdo JSON exposto por um componente AEM pode ser inserido automaticamente em um componente React como props.

## Objetivo

1. Saiba como mapear componentes AEM para SPA componentes.
2. Entenda a diferença entre os componentes **Container** e **Content**.
3. Crie um novo componente React que mapeie para um componente AEM existente.

## O que você vai criar

Este capítulo verificará como o componente `Text` SPA fornecido é mapeado para o componente AEM `Text`. Um novo componente de SPA `Image` será criado e poderá ser usado no SPA e criado no AEM. Os recursos prontos para uso das políticas **Container de layout** e **Editor de modelos** também serão usados para criar uma visualização com uma aparência um pouco mais variada.

![Criação final de amostra de capítulo](./assets/map-components/final-page.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/map-components-start
   ```

2. Implante a base de código para uma instância AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) ou fazer check-out do código localmente ao alternar para a ramificação `React/map-components-solution`.

## Abordagem de mapeamento

O conceito básico é mapear um componente SPA para um componente AEM. AEM componentes, execute o lado do servidor e exporte conteúdo como parte da API do modelo JSON. O conteúdo JSON é consumido pelo SPA, executando o lado do cliente no navegador. Um mapeamento 1:1 entre SPA componentes e um componente AEM é criado.

![Visão geral de alto nível do mapeamento de um componente AEM para um componente React](./assets/map-components/high-level-approach.png)

*Visão geral de alto nível do mapeamento de um componente AEM para um componente React*

## Inspect o componente de texto

O [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) fornece um componente `Text` que está mapeado para o AEM [componente de Texto](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/text.html). Este é um exemplo de um componente **content**, na medida em que renderiza *content* do AEM.

Vamos ver como o componente funciona.

### Inspect o modelo JSON

1. Antes de pular para o código SPA, é importante entender o modelo JSON que AEM fornece. Navegue até [Biblioteca de componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html) e visualização a página do componente de Texto. A Biblioteca de componentes principais fornece exemplos de todos os componentes principais AEM.
2. Selecione a guia **JSON** para um dos exemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Você deve ver três propriedades: `text`, `richText` e `:type`.

   `:type` é uma propriedade reservada que lista o  `sling:resourceType` (ou caminho) do Componente AEM. O valor de `:type` é o que é usado para mapear o componente AEM para o componente SPA.

   `text` e  `richText` são propriedades adicionais que serão expostas ao componente SPA.

### Inspect do componente Texto

1. Abra um novo terminal e navegue até a pasta `ui.frontend` dentro do projeto. Execute `npm install` e `npm start` para start o **webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

   O módulo `ui.frontend` está configurado para usar o [modelo JSON](./integrate-spa.md#mock-json).

2. Você deverá ver uma nova janela do navegador aberta para [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Servidor de desenvolvimento do Webpack com conteúdo mock](./assets/map-components/initial-start.png)

3. No IDE de sua escolha, abra o AEM Project para a SPA WKND. Expanda o módulo `ui.frontend` e abra o arquivo `Text.js` em `ui.frontend/src/components/Text/Text.js`:

   ![Código fonte do componente React.js](./assets/map-components/vscode-ide-text-js.png)

4. A primeira área que inspecionaremos será a `class Text` em ~linha 40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` é um componente React padrão. O componente usa `this.props.richText` para determinar se o conteúdo a ser renderizado será Rich Text ou Texto sem formatação. O &quot;conteúdo&quot; real usado vem de `this.props.text`. Para evitar um possível ataque XSS, o texto rico é escapado via `DOMPurify` antes de usar [perigosamenteSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) para renderizar o conteúdo. Lembre-se das propriedades `richText` e `text` do modelo JSON anteriormente no exercício.

5. Em seguida, dê uma olhada no `TextEditConfig` em ~linha 29:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   O código acima é responsável por determinar quando renderizar o espaço reservado no ambiente do autor AEM. Se o método `isEmpty` retornar **true**, o espaço reservado será renderizado.

6. Por fim, veja a chamada `MapTo` em ~linha 62:

   ```js
   export default MapTo('wknd-spa-react/components/text')(Text, TextEditConfig);
   ```

   `MapTo` é fornecido pelo AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`). O caminho `wknd-spa-react/components/text` representa `sling:resourceType` do componente AEM. Esse caminho é correspondido com o `:type` exposto pelo modelo JSON observado anteriormente. `MapTo` trata da análise da resposta do modelo JSON e da transmissão dos valores corretos como  `props` o componente SPA.

   Você pode encontrar a definição do componente AEM `Text` em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

7. Experimente modificando o arquivo `mock.model.json` em `ui.frontend/public/mock-content/mock.model.json`. Em ~line 62, atualize o primeiro valor `Text` para usar as tags **`H1`** e **`u`**:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       }
   ```

   Navegue até [http://localhost:3000](http://localhost:3000) para ver os efeitos:

   ![Modelo de texto atualizado](./assets/map-components/updated-text-model.png)

   Tente alternar a propriedade `richText` entre **true** / **false** para ver a lógica de renderização em ação.

8. Inspect `Text.scss` em `ui.frontend/src/components/Text/Text.scss`.

   Este arquivo foi adicionado pela base de código inicial deste capítulo e utiliza o recurso [Sass](https://sass-lang.com/) adicionado no capítulo anterior. Observe as variáveis referenciadas de `ui.frontend/src/styles/_variables.scss`.

## Criar o componente de imagem

Em seguida, crie um componente `Image` React que esteja mapeado para o AEM [Componente de imagem](https://docs.adobe.com/content/help/br/experience-manager-core-components/using/components/image.html). O componente `Image` é outro exemplo de um componente **content**.

### Inspect o JSON

Antes de pular para o código SPA, inspecione o modelo JSON fornecido pela AEM.

1. Navegue até [Exemplos de imagem na biblioteca de componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/image.html).

   ![Componente principal de imagem JSON](./assets/map-components/image-json.png)

   As propriedades de `src`, `alt` e `title` serão usadas para preencher o componente SPA `Image`.

   >[!NOTE]
   >
   > Há outras propriedades de Imagem expostas (`lazyEnabled`, `widths`) que permitem que um desenvolvedor crie um componente adaptativo e com carregamento lento. O componente criado neste tutorial será simples e **não** usará essas propriedades avançadas.

2. Retorne ao seu IDE e abra o `mock.model.json` em `ui.frontend/public/mock-content/mock.model.json`. Como este é um componente novo para nosso projeto, precisamos &quot;zombar&quot; do Image JSON.

   Na ~linha 70, adicione uma entrada JSON para o modelo `image` (não se esqueça da vírgula à direita `,` após a segunda `text_23828680`) e atualize a matriz `:itemsOrder`.

   ```json
   ...
   ":items": {
               ...
               "text_23828680": {
                   "text": "<p>Mock Model JSON!</p>",
                   "richText": true,
                   ":type": "wknd-spa-react/components/text"
               },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mock-content/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-react/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_23828680",
               "image"
           ],
   ```

   O projeto inclui uma imagem de amostra em `/mock-content/adobestock-140634652.jpeg` que será usada com o **webpack-dev-server**.

   Você pode visualização o [mock.model.json completo aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/public/mock-content/mock.model.json).

### Implementar o componente de Imagem

1. Em seguida, crie uma nova pasta chamada `Image` em `ui.frontend/src/components`.
2. Abaixo da pasta `Image` crie um novo arquivo chamado `Image.js`.

   ![Arquivo Image.js](./assets/map-components/image-js-file.png)

3. Adicione as seguintes instruções `import` a `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

4. Em seguida, adicione `ImageEditConfig` para determinar quando mostrar o espaço reservado no AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   O espaço reservado mostrará se a propriedade `src` não estiver definida.

5. Em seguida, implemente a classe `Image`:

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   O código acima renderizará um `<img>` com base nos props `src`, `alt` e `title` passados pelo modelo JSON.

6. Adicione o código `MapTo` para mapear o componente React para o componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Observe que a string `wknd-spa-react/components/image` corresponde ao local do componente AEM em `ui.apps` em: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

7. Crie um novo arquivo chamado `Image.scss` no mesmo diretório e adicione o seguinte:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

8. Em `Image.js`, adicione uma referência ao arquivo na parte superior abaixo das instruções `import`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.scss');
   ```

   Você pode visualização o [Image.js concluído aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/React/map-components-solution/ui.frontend/src/components/Image/Image.js).

9. Abra o arquivo `ui.frontend/src/components/import-components.js` e adicione uma referência ao novo componente `Image`:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Image/Image'; //add reference to Image component
   ```

10. Se ainda não tiver sido iniciado, start **webpack-dev-server**. Navegue até [http://localhost:3000](http://localhost:3000) e você deverá ver a renderização da imagem:

   ![Imagem adicionada à maquete](./assets/map-components/image-added-mock.png)

   >[!NOTE]
   >
   > **Desafio** de bônus: Implemente um novo método em  `Image.js` para exibir o valor de  `this.props.title` como uma legenda abaixo da imagem.

## Atualizar políticas em AEM

O componente `Image` só está visível no **webpack-dev-server**. Em seguida, implante o SPA atualizado para AEM e atualizar as políticas de modelo.

1. Pare o **webpack-dev-server** e da raiz do projeto, implemente as alterações para AEM usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Na tela AEM Start, navegue até **Ferramentas** > **Modelos** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

   Selecione e edite a **Página SPA**:

   ![Editar modelo de página SPA](./assets/map-components/edit-spa-page-template.png)

3. Selecione o Container **Layout** e clique no ícone **policy** para editar a política:

   ![Política de Container de layout](./assets/map-components/layout-container-policy.png)

4. Em **Componentes permitidos** > **SPA WKND React - Content** > verifique o componente **Image**:

   ![Componente de imagem selecionado](./assets/map-components/check-image-component.png)

   Em **Componentes Padrão** > **Adicionar mapeamento** e escolha o componente **Imagem - SPA WKND Reagir - Conteúdo**:

   ![Definir componentes padrão](./assets/map-components/default-components.png)

   Digite um **tipo mime** de `image/*`.

   Clique em **Concluído** para salvar as atualizações de política.

5. No **Container de layout** clique no ícone **policy** para o componente **Text**:

   ![Ícone de política de componente de texto](./assets/map-components/edit-text-policy.png)

   Crie uma nova política chamada **Texto SPA WKND**. Em **Plug-ins** > **Formatação** > marque todas as caixas para ativar opções de formatação adicionais:

   ![Ativar formatação RTE](assets/map-components/enable-formatting-rte.png)

   Em **Plug-ins** > **Estilos de parágrafo** > marque a caixa de seleção **Ativar estilos de parágrafo**:

   ![Ativar estilos de parágrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clique em **Concluído** para salvar a atualização da política.

6. Navegue até **Página inicial** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

   Você também pode editar o componente `Text` e adicionar estilos de parágrafo adicionais no modo **tela cheia**.

   ![Edição de Rich Text em Tela Cheia](assets/map-components/full-screen-rte.png)

7. Você também pode arrastar e soltar uma imagem do **Localizador de ativos**:

   ![Arrastar e soltar imagem](./assets/map-components/drag-drop-image.gif)

8. Adicione suas próprias imagens via [AEM Assets](http://localhost:4502/assets.html/content/dam) ou instale a base de código finalizada para o site de referência padrão [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). O [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) inclui muitas imagens que podem ser reutilizadas no SPA WKND. O pacote pode ser instalado usando [AEM Gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp).

   ![O gerenciador de pacotes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect o Container Layout

O suporte para **Container de layout** é fornecido automaticamente pelo AEM SPA Editor SDK. O container **Layout**, conforme indicado pelo nome, é um componente **Container**. Os componentes do container são componentes que aceitam estruturas JSON que representam *outros* componentes e os instanciam dinamicamente.

Vamos inspecionar o Container Layout ainda mais.

1. Em um navegador, navegue até [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API de modelo JSON - Grade responsiva](./assets/map-components/responsive-grid-modeljson.png)

   O componente **Container de layout** tem `sling:resourceType` de `wcm/foundation/components/responsivegrid` e é reconhecido pelo Editor de SPA usando a propriedade `:type`, assim como os componentes `Text` e `Image`.

   Os mesmos recursos de redimensionar um componente usando [Modo de layout](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) estão disponíveis no Editor de SPA.

2. Retorne para [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Adicione outros componentes **Image** e tente redimensioná-los usando a opção **Layout**:

   ![Redimensionar imagem usando o modo Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Reabra o modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e observe `columnClassNames` como parte do JSON:

   ![Nomes de classe da coluna](./assets/map-components/responsive-grid-classnames.png)

   O nome da classe `aem-GridColumn--default--4` indica que o componente deve ter 4 colunas de largura com base em uma grade de 12 colunas. Mais detalhes sobre a grade responsiva [podem ser encontrados aqui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Retorne ao IDE e no módulo `ui.apps` há uma biblioteca do lado do cliente definida em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Abra o arquivo `less/grid.less`.

   Este arquivo determina os pontos de interrupção (`default`, `tablet` e `phone`) usados pelo **Container de layout**. Este arquivo deve ser personalizado de acordo com as especificações do projeto. Atualmente, os pontos de interrupção estão definidos como `1200px` e `650px`.

5. Você deve ser capaz de usar os recursos responsivos e as políticas de rich text atualizadas do componente `Text` para criar uma visualização como a seguinte:

   ![Criação final de amostra de capítulo](./assets/map-components/final-page.png)

## Parabéns! {#congratulations}

Parabéns, você aprendeu a mapear SPA componentes para AEM Componentes e implementou um novo componente `Image`. Você também tem a chance de explorar os recursos responsivos do **Container de layout**.

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/map-components-solution) ou fazer check-out do código localmente ao alternar para a ramificação `React/map-components-solution`.

### Próximas etapas {#next-steps}

[Navegação e Roteamento](navigation-routing.md)  - saiba como várias visualizações no SPA podem ser suportadas ao mapear para AEM páginas com o SPA Editor SDK. A navegação dinâmica é implementada usando o Roteador de Reação e adicionada a um componente de Cabeçalho existente.

## Bônus - Persistir em configurações para o controle de origem {#bonus}

Em muitos casos, especialmente no início de um projeto AEM é importante persistir em configurações, como modelos e políticas de conteúdo relacionadas, para o controle de origem. Isso garante que todos os desenvolvedores estejam trabalhando em relação ao mesmo conjunto de conteúdo e configurações e pode garantir uma consistência adicional entre os ambientes. Quando um projeto atinge um certo nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.

As próximas etapas serão executadas usando o Visual Studio Code IDE e [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), mas podem ser executadas usando qualquer ferramenta e qualquer IDE que você tenha configurado para **pull** ou **importar** conteúdo de uma instância local do AEM.

1. No Visual Studio Code IDE, verifique se você tem **VSCode AEM Sync** instalado por meio da extensão do Marketplace:

   ![Sincronização AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Expanda o módulo **ui.content** no Explorador de projetos e navegue até `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Clique com o botão direito do mouse** na  `templates` pasta e selecione  **Importar do servidor** AEM:

   ![Template de importação VSCode](./assets/map-components/import-aem-servervscode.png)

4. Repita as etapas para importar conteúdo, mas selecione a pasta **policies** localizada em `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect o arquivo `filter.xml` localizado em `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   O arquivo `filter.xml` é responsável por identificar os caminhos dos nós que serão instalados com o pacote. Observe `mode="merge"` em cada um dos filtros que indica que o conteúdo existente não será modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **e não** substitua o conteúdo. Consulte a [documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.
