---
title: Mapear componentes do SPA para componentes do AEM AEM | Introdução ao SPA Editor e React
description: Saiba como mapear componentes do React para componentes do Adobe Experience Manager (AEM) com o SDK JS do editor do AEM SPA. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas nos componentes do SPA no editor SPA AEM, de forma semelhante à criação tradicional do AEM. Você também aprenderá a usar os Componentes principais de reação AEM prontos para uso.
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2257'
ht-degree: 1%

---

# Mapear componentes do SPA para componentes do AEM {#map-components}

Saiba como mapear componentes do React para componentes do Adobe Experience Manager (AEM) com o SDK JS do editor do AEM SPA. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas nos componentes do SPA no editor SPA AEM, de forma semelhante à criação tradicional do AEM.

Este capítulo detalha a API do modelo JSON do AEM e como o conteúdo JSON exposto por um componente AEM pode ser injetado automaticamente em um componente React como props.

## Objetivo

1. Saiba como mapear componentes de AEM para componentes de SPA.
1. Inspect como um componente do React usa propriedades dinâmicas transmitidas do AEM.
1. Saiba como usar o imediatamente [Componentes principais do AEM React](https://github.com/adobe/aem-react-core-wcm-components-examples).

## O que você vai criar

Este capítulo inspeciona como as `Text` O componente SPA é mapeado para o AEM `Text`componente. Reaja os Componentes principais como o `Image` O componente SPA é usado no SPA e é de autoria no AEM. Recursos prontos para uso do **Contêiner de layout** e **Editor de modelo** as políticas também devem ser usadas para criar uma visão um pouco mais variada na aparência.

![Criação final de amostra de capítulo](./assets/map-components/final-page.png)

## Pré-requisitos

Analisar as ferramentas e instruções necessárias para a configuração de um [ambiente de desenvolvimento local](overview.md#local-dev-environment). O presente capítulo é uma continuação do [Integrar o SPA](integrate-spa.md) capítulo, no entanto, tudo o que você precisa é de um projeto AEM habilitado para SPA.

## Abordagem de mapeamento

O conceito básico é mapear um componente SPA para um componente AEM. Componentes do AEM, executar no lado do servidor, exportar conteúdo como parte da API do modelo JSON. O conteúdo JSON é consumido pelo SPA, executando no lado do cliente no navegador. Um mapeamento 1:1 entre componentes SPA e um componente AEM é criado.

![Visão geral de alto nível do mapeamento de um componente AEM para um componente React](./assets/map-components/high-level-approach.png)

*Visão geral de alto nível do mapeamento de um componente AEM para um componente React*

## Inspect, o componente de Texto

A variável [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) fornece uma `Text` componente mapeado para o AEM [Componente de texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Este é um exemplo de **conteúdo** componente, na medida em que *conteúdo* do AEM.

Vamos ver como o componente funciona.

### Inspect e o modelo JSON

1. Antes de pular para o código SPA, é importante entender o modelo JSON que o AEM fornece. Navegue até a [Biblioteca de componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) e visualize a página do componente de Texto. A Biblioteca de componentes principais fornece exemplos de todos os componentes principais AEM.
1. Selecione o **JSON** para um dos exemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Você deve ver três propriedades: `text`, `richText`, e `:type`.

   `:type` é uma propriedade reservada que lista as `sling:resourceType` (ou caminho) do componente AEM. O valor de `:type` é usado para mapear o componente AEM para o componente SPA.

   `text` e `richText` são propriedades adicionais expostas ao componente SPA.

1. Exibir a saída JSON em [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Você poderá encontrar uma entrada semelhante a:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect do componente Texto SPA

1. No IDE de sua escolha, abra o Projeto AEM para o SPA. Expanda a `ui.frontend` e abra o arquivo `Text.js` em `ui.frontend/src/components/Text/Text.js`.

1. A primeira área que vamos inspecionar é a `class Text` em ~linha 40:

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

   Para evitar um possível ataque XSS, o rich text é escapado por `DOMPurify` antes de usar [perigosamenteDefinirHTMLInterno](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) para processar o conteúdo. Lembre-se de `richText` e `text` do modelo JSON anteriormente neste exercício.

1. Próximo, abrir `ui.frontend/src/components/import-components.js` dê uma olhada no `TextEditConfig` em ~linha 86:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   O código acima é responsável por determinar quando renderizar o espaço reservado no ambiente do autor do AEM. Se a variável `isEmpty` o método retorna **true** em seguida, o espaço reservado é renderizado.

1. Por fim, dê uma olhada no `MapTo` chame em ~linha 94:

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` é fornecido pelo SPA Editor de JS SDK do AEM (`@adobe/aem-react-editable-components`). O caminho `wknd-spa-react/components/text` representa o `sling:resourceType` componente AEM. Esse caminho é compatível com o `:type` exposto pelo modelo JSON observado anteriormente. `MapTo` cuida de analisar a resposta do modelo JSON e transmitir os valores corretos como `props` ao componente SPA.

   Você pode encontrar o AEM `Text` definição de componente em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Usar componentes principais do React

[Componentes WCM do AEM - Implementação principal do React](https://github.com/adobe/aem-react-core-wcm-components-base) e [Componentes WCM do AEM - Editor de Spa - Implementação do React Core](https://github.com/adobe/aem-react-core-wcm-components-spa). Trata-se de um conjunto de componentes reutilizáveis da interface do usuário que mapeiam para componentes AEM prontos para uso. A maioria dos projetos pode reutilizar esses componentes como ponto de partida para sua própria implementação.

1. No código do projeto, abra o arquivo `import-components.js` em `ui.frontend/src/components`.
Esse arquivo importa todos os componentes do SPA que são mapeados para componentes do AEM. Dada a natureza dinâmica da implementação do Editor de SPA, devemos fazer referência explícita a quaisquer componentes SPA que estejam vinculados a componentes com autor de AEM. Isso permite que um autor de AEM escolha usar um componente onde quiser no aplicativo.
1. As seguintes declarações de importação incluem componentes SPA gravados no projeto:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Há vários outros `imports` de `@adobe/aem-core-components-react-spa` e `@adobe/aem-core-components-react-base`. Eles estão importando os componentes principais do React e disponibilizando-os no projeto atual. Eles são mapeados para componentes AEM específicos do projeto usando o `MapTo`, assim como com o `Text` exemplo de componente anterior.

### Atualizar políticas de AEM

As políticas são um recurso dos modelos de AEM que fornece aos desenvolvedores e usuários avançados controle granular sobre quais componentes estão disponíveis para serem usados. Os Componentes principais do React estão incluídos no Código SPA, mas precisam ser ativados por meio de uma política antes de serem usados no aplicativo.

1. Na tela inicial do AEM, acesse **Ferramentas** > **Modelos** > **[Reação WKND ao SPA](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Selecione e abra o **Página SPA** modelo para edição.

1. Selecione o **Contêiner de layout** e clique em **política** ícone para editar a política:

   ![política de contêiner de layout](assets/map-components/edit-spa-page-template.png)

1. Em **Componentes permitidos** > **Reação SPA WKND - Conteúdo** > verificar **Imagem**, **Teaser**, e **Título**.

   ![Componentes atualizados disponíveis](assets/map-components/update-components-available.png)

   Em **Componentes padrão** > **Adicionar mapeamento** e escolha o **Imagem - WKND SPA React - Conteúdo** componente:

   ![Definir componentes padrão](./assets/map-components/default-components.png)

   Insira um **tipo mime** de `image/*`.

   Clique em **Concluído** para salvar as atualizações de política.

1. No **Contêiner de layout** clique em **política** ícone para a variável **Texto** componente.

   Crie uma nova política chamada **Texto SPA WKND**. Em **Plug-ins** > **Formatação** > marque todas as caixas para ativar opções adicionais de formatação:

   ![Ativar a formatação RTE](assets/map-components/enable-formatting-rte.png)

   Em **Plug-ins** > **Estilos de parágrafo** > marque a caixa para **Ativar estilos de parágrafo**:

   ![Habilitar estilos de parágrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clique em **Concluído** para salvar a atualização de política.

### Conteúdo do autor

1. Navegue até a **Página inicial** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Agora você poderá usar os componentes adicionais **Imagem**, **Teaser**, e **Título** na página.

   ![Componentes adicionais](assets/map-components/additional-components.png)

1. Também é possível editar a variável `Text` e adicionar outros estilos de parágrafo em **tela cheia** modo.

   ![Edição de Rich Text em Tela Inteira](assets/map-components/full-screen-rte.png)

1. Também é possível arrastar e soltar uma imagem da tag **Localizador de ativos**:

   ![Arrastar e soltar imagem](assets/map-components/drag-drop-image.png)

1. Experiência com o **Título** e **Teaser** componentes.

1. Adicione suas próprias imagens pelo [AEM Assets](http://localhost:4502/assets.html/content/dam) ou instale a base de código concluída para o padrão [Site de referência da WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). A variável [Site de referência da WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) O inclui muitas imagens que podem ser reutilizadas no SPA WKND. O pacote pode ser instalado usando [Gerenciador de pacotes AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Instalação do Gerenciador de pacotes wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect o contêiner de layout

Suporte para o **Contêiner de layout** O é fornecido automaticamente pelo SDK do editor SPA AEM. A variável **Contêiner de layout**, conforme indicado pelo nome, é um **container** componente. Componentes de contêiner são componentes que aceitam estruturas JSON que representam *outro* componentes e instanciá-los dinamicamente.

Vamos analisar mais detalhadamente o Contêiner de layout.

1. Em um navegador, navegue até [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API do modelo JSON - Grade responsiva](./assets/map-components/responsive-grid-modeljson.png)

   A variável **Contêiner de layout** O componente tem um `sling:resourceType` de `wcm/foundation/components/responsivegrid` e é reconhecido pelo Editor de SPA com o `:type` propriedade, exatamente como a `Text` e `Image` componentes.

   Os mesmos recursos de redimensionamento de um componente usando [Modo de layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) estão disponíveis com o Editor de SPA.

2. Retornar para [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Adicionar adicional **Imagem** componentes e tente redimensioná-los usando o **Layout** opção:

   ![Redimensionar imagem usando o modo Layout](./assets/map-components/responsive-grid-layout-change.gif)

3. Reabra o modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e observe as `columnClassNames` como parte do JSON:

   ![Nomes de Classe de Coluna](./assets/map-components/responsive-grid-classnames.png)

   O nome da classe `aem-GridColumn--default--4` indica que o componente deve ter 4 colunas de largura com base em uma grade de 12 colunas. Mais detalhes sobre o [a grade responsiva pode ser encontrada aqui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Retorne ao IDE e no `ui.apps` há uma biblioteca do lado do cliente definida em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Abra o arquivo `less/grid.less`.

   Esse arquivo determina os pontos de interrupção (`default`, `tablet`, e `phone`) usado pelo **Contêiner de layout**. Este arquivo deve ser personalizado de acordo com as especificações do projeto. No momento, os pontos de interrupção estão definidos como `1200px` e `768px`.

5. Você deve poder usar os recursos responsivos e as políticas de rich text atualizadas do `Text` para criar uma exibição como a seguinte:

   ![Criação final de amostra de capítulo](assets/map-components/final-page.png)

## Parabéns. {#congratulations}

Parabéns, você aprendeu a mapear componentes do SPA para componentes do AEM e usou os Componentes principais do React. Você também tem a chance de explorar os recursos responsivos do **Contêiner de layout**.

### Próximas etapas {#next-steps}

[Navegação e Roteamento](navigation-routing.md) - Saiba como várias exibições no SPA podem ser compatíveis com o mapeamento para páginas AEM com o SDK do Editor de SPA. A navegação dinâmica é implementada usando o Roteador React e os Componentes principais do React.

## (Bônus) Configurações persistentes para o controle de origem {#bonus-configs}

Em muitos casos, especialmente no início de um projeto AEM, é valioso manter as configurações, como modelos e políticas de conteúdo relacionadas, no controle de origem. Isso garante que todos os desenvolvedores trabalhem com o mesmo conjunto de conteúdo e configurações e possa garantir consistência adicional entre os ambientes. Quando um projeto atinge um determinado nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.

As próximas etapas ocorrerão usando o Visual Studio Code IDE e [Sincronização VSCode com AEM](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) mas pode estar usando qualquer ferramenta e qualquer IDE que você tenha configurado para **obter** ou **importar** conteúdo de uma instância local do AEM.

1. No Visual Studio Code IDE, certifique-se de que tenha **Sincronização VSCode com AEM** instalado pela extensão do Marketplace:

   ![Sincronização VSCode com AEM](./assets/map-components/vscode-aem-sync.png)

2. Expanda a **ui.content** módulo no Gerenciador de projetos e navegue até `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Clique com o botão direito** o `templates` e selecione **Importar do servidor AEM**:

   ![Modelo de importação de VSCode](./assets/map-components/import-aem-servervscode.png)

4. Repita as etapas para importar conteúdo, mas selecione a variável **políticas** pasta localizada em `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. INSPECT o `filter.xml` arquivo localizado em `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   A variável `filter.xml` O arquivo é responsável por identificar os caminhos dos nós instalados com o pacote. Observe a `mode="merge"` em cada filtro que indica que o conteúdo existente não será modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **não** substituir conteúdo. Consulte a [Documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Comparar `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.

## (Bônus) Criar componente de imagem personalizado {#bonus-image}

Um componente de Imagem SPA já foi fornecido pelos componentes principais do React. No entanto, se quiser praticar mais, crie sua própria implementação do React que mapeia para o AEM [Componente de imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR). A variável `Image` componente é outro exemplo de um **conteúdo** componente.

### Inspect, o JSON

Antes de pular para o código SPA, inspecione o modelo JSON fornecido pelo AEM.

1. Navegue até a [Exemplos de imagem na biblioteca de Componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Componente principal de imagem JSON](./assets/map-components/image-json.png)

   Propriedades de `src`, `alt`, e `title` são usados para preencher o SPA `Image` componente.

   >[!NOTE]
   >
   > Há outras propriedades de imagem expostas (`lazyEnabled`, `widths`) que permitem ao desenvolvedor criar um componente adaptável e de carregamento lento. O componente criado neste tutorial é simples e **não** use essas propriedades avançadas.

### Implementar o componente de Imagem

1. Em seguida, crie uma nova pasta chamada `Image` em `ui.frontend/src/components`.
1. Abaixo de `Image` pasta criar um novo arquivo chamado `Image.js`.

   ![Arquivo Image.js](./assets/map-components/image-js-file.png)

1. Adicione o seguinte `import` instruções para `Image.js`:

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

   O espaço reservado mostrará se a variável `src` propriedade não está definida.

1. Em seguida, implemente o `Image` classe:

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

   O código acima renderizará um `<img>` com base nas props `src`, `alt`, e `title` passado pelo modelo JSON.

1. Adicione o `MapTo` código para mapear o componente React ao componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Anote a string `wknd-spa-react/components/image` corresponde à localização do componente AEM no `ui.apps` em: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Crie um novo arquivo chamado `Image.css` no mesmo diretório e adicione o seguinte:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Entrada `Image.js` adicione uma referência ao arquivo na parte superior abaixo de `import` instruções:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Abra o arquivo `ui.frontend/src/components/import-components.js` e adicionar uma referência ao novo `Image` componente:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Entrada `import-components.js` comente a imagem do componente principal do React:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Isso garantirá que nosso componente de Imagem personalizado seja usado no lugar.

1. Na raiz do projeto, implante o código SPA no AEM usando Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect o SPA no AEM. Todos os componentes de Imagem na página devem continuar a funcionar. O Inspect renderizou a saída e você deve ver a marcação do componente de Imagem personalizado em vez do componente principal do React.

   *Marcação do componente de Imagem personalizada*

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
