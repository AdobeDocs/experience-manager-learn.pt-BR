---
title: Bibliotecas do lado do cliente e fluxo de trabalho do front-end
description: Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar CSS e Javascript para uma implementação do Adobe Experience Manager (AEM) Sites. Este tutorial também abordará como o módulo ui.front-end, um projeto de webpack, pode ser integrado ao processo de compilação completo.
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 5%

---


# Bibliotecas do lado do cliente e fluxo de trabalho do front-end {#client-side-libraries}

Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar CSS e Javascript para uma implementação do Adobe Experience Manager (AEM) Sites. Este tutorial também abordará como o módulo [ui.frontenda](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) , um projeto desacoplado do [webpack](https://webpack.js.org/) , pode ser integrado ao processo de criação completo.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um ambiente [de desenvolvimento](overview.md#local-dev-environment)local.

Também é recomendável revisar o tutorial [Component Basics](component-basics.md#client-side-libraries) para entender os fundamentos das bibliotecas e AEM do cliente.

### Projeto inicial

Confira o código básico no qual o tutorial se baseia:

1. Clonar o repositório [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) .
1. Confira o `client-side-libraries/start` ramo

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Implante a base de código para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Você sempre pode visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) ou fazer check-out do código localmente ao alternar para a ramificação `client-side-libraries/solution`.

## Objetivo

1. Entenda como as bibliotecas do lado do cliente são incluídas em uma página por meio de um modelo editável.
1. Saiba como usar o Módulo UI.Fronend e um servidor de desenvolvimento de webpack para o desenvolvimento de front-end dedicado.
1. Entenda o fluxo de trabalho completo de fornecer CSS e JavaScript compilados para uma implementação do Sites.

## O que você vai criar {#what-you-will-build}

Neste capítulo, você adicionará alguns estilos de linha de base para o site da WKND e o Modelo de página de artigo, a fim de aproximar a implementação dos modelos de design da [interface do usuário](assets/pages-templates/wknd-article-design.xd). Você usará um fluxo de trabalho avançado de front-end para integrar um projeto de webpack a uma biblioteca de clientes AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Segundo plano {#background}

As bibliotecas do lado do cliente fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As metas básicas para bibliotecas do lado do cliente ou clientlibs são:

1. Armazenar CSS/JS em pequenos arquivos discretos para facilitar o desenvolvimento e a manutenção
1. Gerenciar dependências em estruturas de terceiros de forma organizada
1. Minimize o número de solicitações do lado do cliente concatenando CSS/JS em uma ou duas solicitações.

Mais informações sobre o uso das bibliotecas do lado do [cliente podem ser encontradas aqui.](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/introduction/clientlibs.translate.html)

As bibliotecas do lado do cliente têm algumas limitações. Mais notável é o suporte limitado para idiomas de front-end populares, como Sass, LESS e TypeScript. No tutorial, veremos como o módulo **ui.frontenda** pode ajudar a resolver isso.

Implante a base do código inicial para uma instância AEM local e navegue até [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). No momento, essa página não tem estilo. Em seguida, implementaremos bibliotecas do lado do cliente para a marca WKND para adicionar CSS e Javascript à página.

## Organização de bibliotecas do lado do cliente {#organization}

Em seguida, exploraremos a organização de clientlibs gerada pelo [AEM Project Archetype](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html).

![Organização de biblioteca de cliente de alto nível](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nível Organização da biblioteca do lado do cliente e inclusão da página*

>[!NOTE]
>
> A seguinte organização de biblioteca do lado do cliente é gerada pelo AEM Project Archetype, mas representa apenas um ponto de partida. Como um projeto gerencia e fornece CSS e Javascript para uma implementação do Sites pode variar drasticamente com base em recursos, habilitações e requisitos.

1. Usando o Eclipse ou outro IDE, abra o módulo **ui.apps** .
1. Expanda o caminho `/apps/wknd/clientlibs` para visualização das clientlibs geradas pelo tipo de arquétipo.

   ![Clientlibs em ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspecionaremos esses clientlibs com mais detalhes abaixo.

1. Inspect as propriedades de `clientlibs/clientlib-base`.

   **clientlib-base** representa o nível básico de CSS e JavaScript necessário para o funcionamento do site WKND. Observe a propriedade `categories` definida como `wknd.base`. `categories` é um mecanismo de marcação para clientlibs e é como eles podem ser referenciados.

   Observe a `embed` propriedade e a `String[]` lista de valores. A `embed` propriedade incorpora outras clientlibs com base em sua categoria. **clientlib-base** incluirá todas as bibliotecas de clientes do Componente principal AEM necessárias. Isso inclui artefatos como javascript para o carrossel, componentes de pesquisa rápida para funcionar. **clientlib-base** não incluirá nenhum CSS e Javascript próprio, mas apenas incorporará outras bibliotecas clientes. **clientlib-base** incorpora a clientlib-grid **clientlib à categoria de** `wknd.grid`.

   Observe a `allowProxy` propriedade definida como `true`. É uma prática recomendada sempre definir `allowProxy=true` em clientlibs. A `allowProxy` propriedade permite armazenar os clientlibs com o código do aplicativo em `/apps` mas **, em seguida, entrega os clientlibs sobre um caminho prefixado com o** `/etc.clientlibs` objetivo de evitar a exposição de qualquer código de aplicativo aos usuários finais. More information about the [allowProxy property can be found here.](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/introduction/clientlibs.translate.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspect as propriedades de `clientlibs/clientlib-grid`.

   **clientlib-grid** é responsável por incluir/gerar o CSS necessário para que o modo [](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/authoring/siteandpage/responsive-layout.translate.html) Layout funcione com o editor do AEM Sites. **clientlib-grid** tinha uma categoria definida como `wknd.grid` e é incorporada via **clientlib-base**.

   A grade pode ser personalizada para usar quantidades diferentes de colunas e pontos de interrupção. Em seguida, atualizaremos os pontos de interrupção padrão gerados.

1. Atualize o arquivo `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   Isso alterará os pontos de interrupção para corresponder aos pontos de interrupção do modelo definidos em `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`.

   Observe que esse arquivo realmente faz referência a um `grid_base.less` arquivo sob o `/libs` qual há uma combinação personalizada para gerar a grade.

1. Inspect as propriedades para `clientlibs/clientlib-site`.

   **o site** clientlib conterá todos os estilos específicos do site para a Marca WKND. Observe a categoria de `wknd.site`. O CSS e o Javascript que geram essa clientlib serão realmente mantidos no `ui.frontend` módulo. A seguir, exploraremos essa integração.

1. Inspect as propriedades para `clientlibs/clientlib-dependencies`.

   **as dependências** clientlib destinam-se a incorporar quaisquer dependências de terceiros. É uma clientlib separada para que possa ser carregada na parte superior da página HTML, se necessário. Observe a categoria de `wknd.dependencies`. O CSS e o Javascript que geram essa clientlib serão realmente mantidos no `ui.frontend` módulo. Exploraremos essa integração posteriormente no tutorial.

## Uso do módulo ui.frontenda {#ui-frontend}

Em seguida, exploraremos o uso do módulo **[ui.frontenda](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** .

### Motivação

As bibliotecas do lado do cliente têm algumas limitações quando se trata de suporte a idiomas como o [Sass](https://sass-lang.com/) ou o [TypeScript](https://www.typescriptlang.org/). Houve também uma explosão de ferramentas de código aberto, como o [NPM](https://www.npmjs.com/) e o [webpack](https://webpack.js.org/) , que aceleram e otimizam o desenvolvimento de front-end.

A ideia básica por trás do módulo **ui.front** é ser capaz de usar excelentes ferramentas como o NPM e o Webpack para gerenciar a maioria do desenvolvimento front-end. Uma peça chave de integração incorporada ao módulo **ui.frontende** , o gerador [aem-clientlib](https://github.com/wcm-io-frontend/aem-clientlib-generator) obtém os artefatos CSS e JS compilados de um projeto webpack/npm e os transforma em bibliotecas AEM do cliente. Isso dá a um desenvolvedor de front-end maior liberdade para escolher diferentes ferramentas e tecnologias.

![integração da arquitetura ui.frontender](assets/client-side-libraries/ui-frontend-architecture.png)

### Uso

Agora adicionaremos alguns estilos básicos para a marca WKND adicionando alguns arquivos Sass (`.scss` extensão) por meio do módulo **ui.frontenda** .

1. Abra o módulo **ui.front** e navegue até `src/main/webpack/base/sass`.

   ![módulo ui.frontenda](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Create a new file named `_variables.scss` beneath the folder `src/main/webpack/base/sass`.
1. Preencha `_variables.scss` com o seguinte:

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   O Sass nos permite criar variáveis, que podem ser usadas em arquivos diferentes para garantir a consistência. Observe as famílias de fontes. Posteriormente, no tutorial, veremos como incluir uma chamada para fontes da Web do Google, para usar essas fontes.

1. Crie outro arquivo com o nome `_elements.scss` abaixo `src/main/webpack/base/sass` e preencha-o com o seguinte:

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   Observe que o `_elements.scss` arquivo usa as variáveis no `_variables.scss`.

1. Atualize `_shared.scss` abaixo `src/main/webpack/base/sass` para incluir os arquivos `_elements.scss` e `_variables.scss` .

   ```css
   @import './variables';
   @import './elements';
   ```

1. Abra um terminal de linha de comando e instale o módulo **ui.frontender** usando o `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` só precisa ser executado uma vez, após um novo clone ou geração do projeto.

1. No mesmo terminal, crie e implante o módulo **ui.frontenda** usando o `npm run dev` comando:

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   O comando `npm run dev` deve criar e compilar o código fonte do projeto do Webpack e, em última análise, preencher as dependências **clientlib-site** e **clientlib no módulo** ui.apps **** .

   >[!NOTE]
   >
   >Há também um `npm run prod` perfil que minimizará o JS e o CSS. Esta é a compilação padrão sempre que a compilação do webpack é acionada via Maven. More details about the [ui.frontend module can be found here](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect o arquivo `site.css` abaixo `ui.frontend/dist/clientlib-site/css/site.css`. Observe que o CSS é composto principalmente de conteúdos do arquivo criado anteriormente, mas as variáveis foram substituídas por valores reais. `_elements.scss`

   ![Css do site distribuído](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect o arquivo `ui.frontend/clientlib.config.js`. Este é o arquivo de configuração para um plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-generator** é a ferramenta responsável por transformar o CSS/JavaScript compilado e copiá-lo para o módulo **ui.apps** .

1. Inspect o arquivo `site.css` no módulo **ui.apps** em `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Essa deve ser uma cópia idêntica do `site.css` arquivo do módulo **ui.frontenda** . Agora que está no módulo **ui.apps** , ele pode ser implantado no AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Como o **clientlib-site** é compilado durante o tempo de criação, usando **npm** ou **maven**, ele pode ser ignorado do controle de origem no módulo **ui.apps** . Inspect o `.gitignore` arquivo abaixo de **ui.apps**.

>[!CAUTION]
>
> O uso do módulo **ui.frontende** pode não ser necessário para todos os projetos. O módulo **ui.front-end** adiciona complexidade adicional e, se não houver necessidade/desejo de usar algumas dessas ferramentas avançadas de front-end (Sass, webpack, npm...), ele pode ser exagerado. Por essa razão, é considerada uma parte opcional do AEM Project Archetype e o uso de bibliotecas padrão do cliente e o vanilla CSS e o JavaScript continua sendo totalmente suportado.

## Inclusão de página e modelo {#page-inclusion}

Em seguida, vamos rever como o projeto é configurado para incluir os clientlibs em AEM modelos/páginas. Uma prática recomendada comum no desenvolvimento da Web é incluir o CSS no cabeçalho HTML `<head>` e no JavaScript antes de fechar a `</body>` tag .

1. In the **ui.apps** module navigate to `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Componente da Página de Estrutura](assets/client-side-libraries/customheaderlibs-html.png)

   Este é o `page` componente usado para renderizar todas as páginas na implementação do WKND.

1. Open the file `customheaderlibs.html`. Observe as linhas `${clientlib.css @ categories='wknd.base'}`. Isso indica que o CSS para a clientlib com uma categoria de `wknd.base` será incluído por meio desse arquivo, incluindo efetivamente **clientlib-base** no cabeçalho de todas as páginas.

1. Atualize `customheaderlibs.html` para incluir uma referência aos estilos de fonte do Google que especificamos anteriormente no módulo **ui.frontenda** . Também vamos comentar o ContextHub por enquanto...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect o arquivo `customfooterlibs.html`. Este arquivo, como `customheaderlibs.html` deve ser substituído pela implementação de projetos. Aqui a linha `${clientlib.js @ categories='wknd.base'}` significa que o JavaScript da base do **clientlib** será incluído na parte inferior de todas as páginas.

1. Crie e implante o projeto em uma instância AEM local usando Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navegue até os modelos WKND em [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Selecione e abra o Modelo **de página de** artigo no Editor de modelos.

   ![Selecionar modelo de página do artigo](assets/client-side-libraries/open-article-page-template.png)

1. Clique no ícone Informações **da** página e, no menu, selecione Política **de** página para abrir a caixa de diálogo Política **de** página.

   ![Política de Página do Menu de Modelo de Página do Artigo](assets/client-side-libraries/template-page-policy.png)

   *Informações da página > Política da página*

1. Observe que as categorias para `wknd.dependencies` e `wknd.site` estão listadas aqui. Por padrão, os clientlibs configurados por meio da Política de página são divididos para incluir o CSS no cabeçalho da página e o JavaScript no final do corpo. Se desejar, você pode lista explicitamente que o clientlib JavaScript seja carregado no cabeçalho da Página. É o que acontece com `wknd.dependencies`.

   ![Política de Página do Menu de Modelo de Página do Artigo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Também é possível fazer referência ao componente da página `wknd.site` ou `wknd.dependencies` do componente da página diretamente, usando o script `customheaderlibs.html` ou `customfooterlibs.html` , como vimos anteriormente para a `wknd.base` clientlib. O uso do modelo oferece alguma flexibilidade para que você possa escolher quais clientlibs são usados por modelo. Por exemplo, se você tiver uma biblioteca JavaScript muito pesada que só será usada em um modelo selecionado.

1. Navegue até a página **LA Skatepark** criada usando o modelo **de página do** artigo: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Você deve observar uma diferença nas fontes e em alguns estilos básicos aplicados para indicar que o CSS criado no módulo **ui.frontenda** está funcionando.

1. Clique no ícone Informações **da** página e, no menu, selecione **Visualização como publicada** para abrir a página do artigo fora do editor de AEM.

   ![Exibir como publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualização a fonte da Página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e você deve ser capaz de ver as seguintes referências clientlib na `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   Observe que os clientlibs estão usando o `/etc.clientlibs` endpoint proxy. Você também deve ver a seguinte clientlib incluída na parte inferior da página:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >É importante no lado da publicação que as bibliotecas do cliente **não** sejam fornecidas de **/aplicativos** , pois esse caminho deve ser restrito por motivos de segurança usando a seção [de filtro do](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)Dispatcher. A propriedade [](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/introduction/clientlibs.translate.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) allowProxy da biblioteca do cliente garante que o CSS e o JS sejam fornecidos em **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

Nos exercícios anteriores, pudemos atualizar vários arquivos Sass no módulo **ui.frontende** e, por meio de um processo de criação, ver essas mudanças refletidas no AEM. Em seguida, vamos analisar como aproveitar um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desenvolver rapidamente nossos estilos de front-end.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Abaixo estão as etapas de alto nível mostradas no vídeo:

1. Start o servidor de desenvolvimento de webpack executando o seguinte comando no módulo **ui.frontender** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Isso deve abrir uma nova janela do navegador em [http://localhost:8080/](http://localhost:8080/) com marcação estática.
1. Copie a fonte da página da página de artigo do LA skatepark em [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Cole a marcação copiada do AEM no módulo `index.html` ui.front **abaixo** `src/main/webpack/static`.
1. Edite a marcação copiada e remova quaisquer referências a dependências de **clientlib-site** e **clientlib**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Podemos remover essas referências porque o servidor dev de webpack gerará esses artefatos automaticamente.

1. Edite os `.scss` arquivos e veja as alterações automaticamente refletidas no navegador.
1. Revise o `/aem-guides-wknd.ui.frontend/webpack.dev.js` arquivo. Contém a configuração do webpack usada para start do servidor webpack-dev. Observe que ele faz proxy dos caminhos `/content` e `/etc.clientlibs` de uma instância de AEM executada localmente. É assim que as imagens e outros clientlibs (não gerenciados pelo código **ui.frontenda** ) são disponibilizados.

   >[!CAUTION]
   >
   > A imagem src da marcação estática aponta para um componente de imagem ao vivo em uma instância de AEM local. As imagens aparecerão quebradas se o caminho para a imagem for alterado, se AEM não for iniciado ou se o navegador usar não tiver feito logon na instância AEM local.
1. Você pode **parar** o servidor do webpack da linha de comando digitando `CTRL+C`.

## Juntando {#putting-it-together}

O foco deste tutorial é nas bibliotecas do lado do cliente e em workflows front-end potenciais para integração com AEM. Com isso em mente, aceleraremos a implementação instalando as bibliotecas do lado do [cliente-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip), que fornece alguns estilos padrão para os componentes principais usados no modelo da página do artigo:

* [Caminho](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/breadcrumb.html)
* [Download](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Imagem](https://docs.adobe.com/content/help/br/experience-manager-core-components/using/components/image.html)
* [Lista](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/list.html)
* [Navegação](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/navigation.html)
* [Busca rápida](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/quick-search.html)
* [Separador](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Abaixo estão as etapas de alto nível mostradas no vídeo:

1. Baixe o arquivo [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) e descompacte o conteúdo abaixo `ui.frontend/src/main/webpack`. O conteúdo do zip deve substituir as seguintes pastas:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. Pré-visualização os novos estilos usando o servidor dev de webpack:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Implante a base de código para uma instância de AEM local para ver os novos estilos aplicados ao artigo do parque de skate LA:

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## Parabéns! {#congratulations}

Parabéns, a página do artigo agora tem alguns estilos consistentes que correspondem à marca WKND e você se familiarizou com o módulo **ui.frontende** !

### Próximas etapas {#next-steps}

Saiba mais sobre como implementar estilos individuais e usar os Componentes principais usando o Sistema de estilo Experience Manager. [O desenvolvimento com o Sistema](style-system.md) de estilo abrange o uso do Sistema de estilo para estender os Componentes principais com CSS específico da marca e configurações avançadas de política do Editor de modelos.

Visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `client-side-libraries/solution`.

1. Clonar o repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) .
1. Confira o `client-side-libraries/solution` galho.

## Ferramentas e recursos adicionais {#additional-resources}

### alimentado {#develop-aemfed}

[**aemfeed**](https://aemfed.io/) é uma ferramenta de linha de comando de código aberto que pode ser usada para acelerar o desenvolvimento de front-end. Ele é acionado por [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Em um nível alto, **aemfeed** foi projetado para acompanhar as alterações de arquivos no módulo **ui.apps** e sincronizá-las automaticamente diretamente em uma instância AEM em execução. Com base nas alterações, um navegador local será atualizado automaticamente, acelerando assim o desenvolvimento de front-end. Ele também foi desenvolvido para trabalhar com o Sling Log tracer para exibir automaticamente quaisquer erros do lado do servidor diretamente no terminal.

Se você estiver trabalhando muito no módulo **ui.apps** , modificando scripts HTL e criando componentes personalizados, o **aemfeed** pode ser uma ferramenta muito poderosa para usar. [A documentação completa pode ser encontrada aqui.](https://github.com/abmaonline/aemfed).

### Depuração de bibliotecas do cliente {#debugging-clientlibs}

Com diferentes métodos de **categoria** e **incorporações** para incluir várias bibliotecas de clientes, pode ser difícil solucionar problemas. AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é a **reconstrução de bibliotecas** de clientes que forçará a AEM a recompilar quaisquer arquivos MENOS e a gerar o CSS.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista todas as bibliotecas clientes registradas na instância AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Saída**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) de teste - permite que um usuário veja a saída HTML esperada de clientlib include com base na categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validação**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) de dependências de bibliotecas - realça quaisquer dependências ou categorias incorporadas que não possam ser encontradas. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Recriar bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) de clientes - permite que um usuário force AEM reconstruir todas as bibliotecas de clientes ou invalidar o cache das bibliotecas de clientes. Essa ferramenta é particularmente eficaz ao desenvolver com MENOS, pois pode forçar AEM a recompilar o CSS gerado. Em geral, é mais eficaz invalidar caches e, em seguida, executar uma atualização de página em vez de recriar todas as bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruir biblioteca do cliente](assets/client-side-libraries/rebuild-clientlibs.png)
