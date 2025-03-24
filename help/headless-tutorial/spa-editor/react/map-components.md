---
title: Mapear componentes de SPA para componentes do AEM | Introdução ao AEM SPA Editor e React
description: Saiba como mapear componentes do React para componentes do Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas em componentes SPA no Editor SPA do AEM, de modo semelhante à criação tradicional no AEM. Você também aprenderá a usar os Componentes principais do AEM React prontos para uso.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
duration: 477
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2123'
ht-degree: 0%

---

# Mapear componentes de SPA para componentes do AEM {#map-components}

Saiba como mapear componentes do React para componentes do Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas em componentes SPA no Editor SPA do AEM, de modo semelhante à criação tradicional no AEM.

Este capítulo detalha a API do modelo JSON do AEM e mostra como o conteúdo JSON exposto por um componente do AEM pode ser inserido automaticamente em um componente do React como props.

## Objetivo

1. Saiba como mapear componentes do AEM para componentes SPA.
1. Inspecione como um componente do React usa propriedades dinâmicas transmitidas pelo AEM.
1. Saiba como usar os [Componentes principais do React AEM](https://github.com/adobe/aem-react-core-wcm-components-examples) prontos para uso.

## O que você vai criar

Este capítulo inspeciona como o componente de SPA `Text` fornecido é mapeado para o componente `Text` do AEM. Os Componentes principais do React, como o componente de SPA `Image`, são usados no SPA e criados no AEM. Os recursos prontos para uso das políticas do **Contêiner de layout** e **Editor de modelos** também podem ser usados para criar um modo de exibição um pouco mais variado na aparência.

![Criação final de amostra de capítulo](./assets/map-components/final-page.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Este capítulo é uma continuação do capítulo [Integrar o SPA](integrate-spa.md). No entanto, basta seguir um projeto do AEM habilitado para SPA.

## Abordagem de mapeamento

O conceito básico é mapear um componente de SPA para um componente do AEM. Componentes do AEM, executar no lado do servidor, exportar conteúdo como parte da API do modelo JSON. O conteúdo JSON é consumido pelo SPA, executando no lado do cliente no navegador. Um mapeamento 1:1 entre componentes de SPA e um componente do AEM é criado.

![Visão geral de alto nível do mapeamento de um componente do AEM para um componente do React](./assets/map-components/high-level-approach.png)

*Visão geral de alto nível do mapeamento de um componente do AEM para um componente do React*

## Inspecione o componente de Texto

O [Arquétipo de Projeto do AEM](https://github.com/adobe/aem-project-archetype) fornece um componente `Text` que é mapeado para o [componente de Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) do AEM. Este é um exemplo de um componente **conteúdo**, no qual ele renderiza *conteúdo* do AEM.

Vamos ver como o componente funciona.

### Inspecionar o modelo JSON

1. Antes de pular para o código SPA, é importante entender o modelo JSON que o AEM fornece. Navegue até a [Biblioteca de Componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) e exiba a página do componente de Texto. A Biblioteca de componentes principais fornece exemplos de todos os Componentes principais do AEM.
1. Selecione a guia **JSON** para um dos exemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Você deve ver três propriedades: `text`, `richText` e `:type`.

   `:type` é uma propriedade reservada que lista o `sling:resourceType` (ou caminho) do Componente AEM. O valor de `:type` é o que é usado para mapear o componente AEM para o componente SPA.

   `text` e `richText` são propriedades adicionais expostas ao componente de SPA.

1. Exiba a saída JSON em [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Você poderá encontrar uma entrada semelhante a:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspecione o componente de SPA de texto

1. No IDE de sua escolha, abra o Projeto do AEM para o SPA. Expanda o módulo `ui.frontend` e abra o arquivo `Text.js` em `ui.frontend/src/components/Text/Text.js`.

1. A primeira área que vamos inspecionar é o `class Text` na linha ~40:

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

   `Text` é um componente padrão do React. O componente usa `this.props.richText` para determinar se o conteúdo a ser renderizado será rich text ou texto simples. O &quot;conteúdo&quot; real usado vem de `this.props.text`.

   Para evitar um possível ataque XSS, o rich text é escapado por `DOMPurify` antes de usar [dangerouslySetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) para renderizar o conteúdo. Lembre-se das propriedades `richText` e `text` do modelo JSON anteriormente neste exercício.

1. A seguir, abra `ui.frontend/src/components/import-components.js` e dê uma olhada no `TextEditConfig` em ~linha 86:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   O código acima é responsável por determinar quando renderizar o espaço reservado no ambiente de autor do AEM. Se o método `isEmpty` retornar **true**, o espaço reservado será renderizado.

1. Por fim, observe a chamada `MapTo` em ~line 94:

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` é fornecido pelo AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`). O caminho `wknd-spa-react/components/text` representa o `sling:resourceType` do componente AEM. Esse caminho corresponde ao `:type` exposto pelo modelo JSON observado anteriormente. `MapTo` cuida de analisar a resposta do modelo JSON e transmitir os valores corretos como `props` para o componente SPA.

   Você pode encontrar a definição do componente `Text` do AEM em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Usar componentes principais do React

[Componentes do AEM WCM - Implementação do React Core](https://github.com/adobe/aem-react-core-wcm-components-base) e [Componentes do AEM WCM - Editor de Spa - Implementação do React Core](https://github.com/adobe/aem-react-core-wcm-components-spa). Esses são um conjunto de componentes reutilizáveis da interface do usuário que mapeiam para componentes prontos para uso do AEM. A maioria dos projetos pode reutilizar esses componentes como ponto de partida para sua própria implementação.

1. No código do projeto, abra o arquivo `import-components.js` em `ui.frontend/src/components`.
Esse arquivo importa todos os componentes de SPA mapeados para componentes do AEM. Dada a natureza dinâmica da implementação do Editor de SPA, devemos fazer referência explícita a quaisquer componentes de SPA vinculados a componentes do AEM que possam ser criados. Isso permite que um autor do AEM escolha usar um componente onde quiser no aplicativo.
1. As declarações de importação a seguir incluem componentes SPA gravados no projeto:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Há vários outros `imports` de `@adobe/aem-core-components-react-spa` e `@adobe/aem-core-components-react-base`. Eles estão importando os componentes principais do React e disponibilizando-os no projeto atual. Eles são mapeados para componentes AEM específicos do projeto usando o `MapTo`, exatamente como no exemplo de componente `Text` anterior.

### Atualizar Políticas Do AEM

As políticas são um recurso dos modelos do AEM e fornecem aos desenvolvedores e usuários avançados controle granular sobre quais componentes estão disponíveis para serem usados. Os Componentes principais do React estão incluídos no Código SPA, mas precisam ser ativados por meio de uma política antes de serem usados no aplicativo.

1. Na tela inicial do AEM, navegue até **Ferramentas** > **Modelos** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Selecione e abra o modelo de **Página de SPA** para edição.

1. Selecione o **Contêiner de layout** e clique em seu ícone de **política** para editar a política:

   ![política de contêiner de layout](assets/map-components/edit-spa-page-template.png)

1. Em **Componentes Permitidos** > **WKND SPA React - Conteúdo** > verificar **Imagem**, **Teaser** e **Título**.

   ![Componentes atualizados disponíveis](assets/map-components/update-components-available.png)

   Em **Componentes Padrão** > **Adicionar mapeamento** e escolha o componente **Imagem - WKND SPA React - Conteúdo**:

   ![Definir componentes padrão](./assets/map-components/default-components.png)

   Insira um **tipo MIME** de `image/*`.

   Clique em **Concluído** para salvar as atualizações de política.

1. No **Contêiner de Layout**, clique no ícone **política** para o componente **Texto**.

   Crie uma nova política chamada **WKND SPA Text**. Em **Plugins** > **Formatação** > marque todas as caixas para habilitar opções de formatação adicionais:

   ![Habilitar Formatação RTE](assets/map-components/enable-formatting-rte.png)

   Em **Plug-ins** > **Estilos de parágrafo** >, marque a caixa para **Habilitar estilos de parágrafo**:

   ![Habilitar estilos de parágrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clique em **Concluído** para salvar a atualização de política.

### Conteúdo do autor

1. Navegue até a **Página inicial** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Agora você pode usar os componentes adicionais **Imagem**, **Teaser** e **Título** na página.

   ![Componentes adicionais](assets/map-components/additional-components.png)

1. Você também pode editar o componente `Text` e adicionar outros estilos de parágrafo no modo **tela cheia**.

   ![Edição de Rich Text em Tela Inteira](assets/map-components/full-screen-rte.png)

1. Você também pode arrastar e soltar uma imagem do **Localizador de ativos**:

   ![Arrastar e soltar imagem](assets/map-components/drag-drop-image.png)

1. Experiência com os componentes **Título** e **Teaser**.

1. Adicione suas próprias imagens via [AEM Assets](http://localhost:4502/assets.html/content/dam) ou instale a base de código concluída para o [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) padrão. O [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) inclui muitas imagens que podem ser reutilizadas no SPA do WKND. O pacote pode ser instalado usando o [Gerenciador de Pacotes](http://localhost:4502/crx/packmgr/index.jsp) da AEM.

   ![Instalar wknd.all](./assets/map-components/package-manager-wknd-all.png) do Gerenciador de Pacotes

## Inspecione o contêiner de layout

O suporte para o **Contêiner de layout** é fornecido automaticamente pelo SDK do Editor SPA do AEM. O **Contêiner de Layout**, conforme indicado pelo nome, é um componente **contêiner**. Componentes de contêiner são componentes que aceitam estruturas JSON que representam *outros* componentes e os instanciam dinamicamente.

Vamos analisar mais detalhadamente o Contêiner de layout.

1. Em um navegador, navegue até [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API do modelo JSON - Grade responsiva](./assets/map-components/responsive-grid-modeljson.png)

   O componente **Contêiner de Layout** tem um `sling:resourceType` de `wcm/foundation/components/responsivegrid` e é reconhecido pelo Editor SPA usando a propriedade `:type`, exatamente como os componentes `Text` e `Image`.

   Os mesmos recursos de redimensionamento de um componente usando o [Modo de layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) estão disponíveis com o Editor de SPA.

2. Retorne a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Adicione componentes adicionais de **Imagem** e tente redimensioná-los usando a opção **Layout**:

   ![Redimensionar imagem usando o modo Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Abra novamente o modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e observe o `columnClassNames` como parte do JSON:

   ![Nomes de Classe de Coluna](./assets/map-components/responsive-grid-classnames.png)

   O nome de classe `aem-GridColumn--default--4` indica que o componente deve ter 4 colunas de largura com base em uma grade de 12 colunas. Mais detalhes sobre a [grade responsiva podem ser encontrados aqui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Retorne ao IDE e no módulo `ui.apps` há uma biblioteca do lado do cliente definida em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Abra o arquivo `less/grid.less`.

   Este arquivo determina os pontos de interrupção (`default`, `tablet` e `phone`) usados pelo **Contêiner de Layout**. Este arquivo deve ser personalizado de acordo com as especificações do projeto. Os pontos de interrupção estão definidos como `1200px` e `768px`.

5. Você deve ser capaz de usar os recursos responsivos e as políticas de rich text atualizadas do componente `Text` para criar uma exibição como a seguinte:

   ![Criação final de amostra de capítulo](assets/map-components/final-page.png)

## Parabéns. {#congratulations}

Parabéns, você aprendeu a mapear componentes SPA para componentes AEM e usou os Componentes principais do React. Você também pode explorar os recursos responsivos do **Contêiner de layout**.

### Próximas etapas {#next-steps}

[Navegação e roteamento](navigation-routing.md) - Saiba como é possível oferecer suporte a várias exibições no SPA mapeando para Páginas do AEM com o SDK do Editor de SPA. A navegação dinâmica é implementada usando o Roteador React e os Componentes principais do React.

## (Bônus) Configurações persistentes para o controle de origem {#bonus-configs}

Em muitos casos, especialmente no início de um projeto do AEM, é valioso manter as configurações, como modelos e políticas de conteúdo relacionadas, no controle de origem. Isso garante que todos os desenvolvedores trabalhem com o mesmo conjunto de conteúdo e configurações e possa garantir consistência adicional entre os ambientes. Quando um projeto atinge um determinado nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.

As próximas etapas ocorrerão usando o IDE do Visual Studio Code e o [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), mas pode ser usando qualquer ferramenta e qualquer IDE que você tenha configurado para **extrair** ou **importar** conteúdo de uma instância local do AEM.

1. No Visual Studio Code IDE, verifique se você tem o **VSCode AEM Sync** instalado por meio da extensão do Marketplace:

   ![Sincronização com o AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Expanda o módulo **ui.content** no Gerenciador de projetos e navegue até `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Clique com o botão direito do mouse** na pasta `templates` e selecione **Importar do AEM Server**:

   ![Modelo de importação do VSCode](./assets/map-components/import-aem-servervscode.png)

4. Repita as etapas para importar o conteúdo, mas selecione a pasta **políticas** localizada em `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspecione o arquivo `filter.xml` localizado em `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   O arquivo `filter.xml` é responsável por identificar os caminhos dos nós instalados com o pacote. Observe o `mode="merge"` em cada filtro que indica que o conteúdo existente não será modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **não** substitua o conteúdo. Consulte a [documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.

## (Bônus) Criar componente de imagem personalizado {#bonus-image}

Um componente de Imagem SPA já foi fornecido pelos componentes principais do React. No entanto, se quiser praticar mais, crie sua própria implementação do React que mapeie para o [componente de Imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR) do AEM. O componente `Image` é outro exemplo de um componente **content**.

### Inspecionar o JSON

Antes de pular para o código SPA, inspecione o modelo JSON fornecido pelo AEM.

1. Navegue até os [exemplos de imagem na biblioteca de Componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Componente principal de imagem JSON](./assets/map-components/image-json.png)

   Propriedades de `src`, `alt` e `title` são usadas para popular o componente de SPA `Image`.

   >[!NOTE]
   >
   > Há outras propriedades de Imagem expostas (`lazyEnabled`, `widths`) que permitem a um desenvolvedor criar um componente adaptável e de carregamento lento. O componente compilado neste tutorial é simples e **não** usa essas propriedades avançadas.

### Implementar o componente de Imagem

1. Em seguida, crie uma nova pasta chamada `Image` em `ui.frontend/src/components`.
1. Abaixo da pasta `Image` crie um novo arquivo chamado `Image.js`.

   ![Arquivo Image.js](./assets/map-components/image-js-file.png)

1. Adicionar as seguintes instruções `import` a `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. Em seguida, adicione o `ImageEditConfig` para determinar quando mostrar o espaço reservado no AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   O espaço reservado será exibido se a propriedade `src` não estiver definida.

1. Em seguida, implemente a classe `Image`:

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

   O código acima renderizará um `<img>` com base nas propriedades `src`, `alt` e `title` passadas pelo modelo JSON.

1. Adicione o código `MapTo` para mapear o componente React ao componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Observe que a cadeia de caracteres `wknd-spa-react/components/image` corresponde ao local do componente AEM em `ui.apps` em: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Crie um novo arquivo chamado `Image.css` no mesmo diretório e adicione o seguinte:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Em `Image.js`, adicione uma referência ao arquivo na parte superior abaixo das instruções `import`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Abra o arquivo `ui.frontend/src/components/import-components.js` e adicione uma referência ao novo componente `Image`:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Em `import-components.js`, comente a Imagem do Componente Principal do React:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Isso garantirá que nosso componente de Imagem personalizado seja usado no lugar.

1. Na raiz do projeto, implante o código SPA no AEM usando o Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspecione o SPA no AEM. Todos os componentes de Imagem na página devem continuar a funcionar. Inspecione a saída renderizada e você deverá ver a marcação para o componente de Imagem personalizado em vez do Componente principal de Reação.

   *Marcação de componente de Imagem personalizada*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Marcação da imagem do componente principal do React*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Esta é uma boa introdução à extensão e implementação de seus próprios componentes.
