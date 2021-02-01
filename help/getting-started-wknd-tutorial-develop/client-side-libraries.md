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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3257'
ht-degree: 2%

---


# Bibliotecas do lado do cliente e fluxo de trabalho do front-end {#client-side-libraries}

Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar CSS e Javascript para uma implementação do Adobe Experience Manager (AEM) Sites. Este tutorial também abordará como o módulo [ui.front-end](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html), um projeto desacoplado [webpack](https://webpack.js.org/), pode ser integrado ao processo de compilação completo.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

Também é recomendável revisar o tutorial [Informações básicas sobre componentes](component-basics.md#client-side-libraries) para entender os fundamentos das bibliotecas e AEM do cliente.

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com êxito o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para fazer check-out do projeto inicial.

Confira o código básico no qual o tutorial se baseia:

1. Verifique a ramificação `tutorial/client-side-libraries-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implante a base de código para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou fazer check-out do código localmente ao alternar para a ramificação `tutorial/client-side-libraries-solution`.

## Objetivo

1. Entenda como as bibliotecas do lado do cliente são incluídas em uma página por meio de um modelo editável.
1. Saiba como usar o Módulo UI.Fronend e um servidor de desenvolvimento de webpack para o desenvolvimento de front-end dedicado.
1. Entenda o fluxo de trabalho completo de fornecer CSS e JavaScript compilados para uma implementação do Sites.

## O que você vai criar {#what-you-will-build}

Neste capítulo, você adicionará alguns estilos de linha de base para o site WKND e o Modelo de página de artigo, a fim de aproximar a implementação dos modelos de design [UI](assets/pages-templates/wknd-article-design.xd). Você usará um fluxo de trabalho avançado de front-end para integrar um projeto de webpack a uma biblioteca de clientes AEM.

![Estilos concluídos](assets/client-side-libraries/finished-styles.png)

*Página do artigo com estilos de linha de base aplicados*

## Segundo plano {#background}

As bibliotecas do lado do cliente fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As metas básicas para bibliotecas do lado do cliente ou clientlibs são:

1. Armazenar CSS/JS em pequenos arquivos discretos para facilitar o desenvolvimento e a manutenção
1. Gerenciar dependências em estruturas de terceiros de forma organizada
1. Minimize o número de solicitações do lado do cliente concatenando CSS/JS em uma ou duas solicitações.

Mais informações sobre como usar [Bibliotecas do lado do cliente podem ser encontradas aqui.](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/introduction/clientlibs.translate.html)

As bibliotecas do lado do cliente têm algumas limitações. Mais notável é o suporte limitado para idiomas de front-end populares, como Sass, LESS e TypeScript. No tutorial, veremos como o módulo **ui.frontende** pode ajudar a resolver isso.

Implante a base do código inicial para uma instância AEM local e navegue até [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). No momento, essa página não tem estilo. Em seguida, implementaremos bibliotecas do lado do cliente para a marca WKND para adicionar CSS e Javascript à página.

## Organização de bibliotecas do lado do cliente {#organization}

Em seguida, exploraremos a organização de clientlibs gerados pelo [AEM Project Archetype](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html).

![Organização de biblioteca de cliente de alto nível](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nível Organização da biblioteca do lado do cliente e inclusão da página*

>[!NOTE]
>
> A seguinte organização de biblioteca do lado do cliente é gerada pelo AEM Project Archetype, mas representa apenas um ponto de partida. Como um projeto gerencia e fornece CSS e Javascript para uma implementação do Sites pode variar drasticamente com base em recursos, habilitações e requisitos.

1. Usando o VSCode ou outro IDE, abra o módulo **ui.apps**.
1. Expanda o caminho `/apps/wknd/clientlibs` para visualização dos clientlibs gerados pelo tipo de arquétipo.

   ![Clientlibs em ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspecionaremos esses clientlibs com mais detalhes abaixo.

1. A tabela a seguir resume as bibliotecas do cliente. Mais detalhes sobre [incluindo as bibliotecas de clientes podem ser encontrados aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrição | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nível básico de CSS e JavaScript necessário para que o Site WKND funcione | incorpora libs de cliente do Componente principal |
   | `clientlib-grid` | Gera o CSS necessário para que [Modo de layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) funcione. | É possível configurar pontos de interrupção para dispositivos móveis/tablets aqui |
   | `clientlib-site` | Contém o tema específico do site para o site WKND | Gerado pelo módulo `ui.frontend` |
   | `clientlib-dependencies` | Incorpora qualquer dependência de terceiros | Gerado pelo módulo `ui.frontend` |

1. Observe que `clientlib-site` e `clientlib-dependencies` são ignorados do controle de origem. Isso é feito por design, pois eles serão gerados no momento da criação pelo módulo `ui.frontend`.

## Atualizar estilos básicos {#base-styles}

Em seguida, atualize os estilos básicos definidos no módulo **[ui.frontende](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. Os arquivos no módulo `ui.frontend` gerarão as bibliotecas `clientlib-site` e `clientlib-dependecies` que contêm o tema Site e quaisquer dependências de terceiros.

As bibliotecas do cliente têm algumas limitações quando se trata de suporte de idiomas como [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Existem várias ferramentas de código aberto, como [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/), que aceleram e otimizam o desenvolvimento de front-end. O objetivo do módulo **ui.frontendent** é poder usar essas ferramentas para gerenciar a maioria dos arquivos de origem front-end.

1. Abra o módulo **ui.front-end** e navegue até `src/main/webpack/site`.
1. Abra o arquivo `main.scss`

   ![main.scss - ponto de entrada](assets/client-side-libraries/main-scss.png)

   `main.scss` é o ponto de entrada para todos os arquivos Sass no  `ui.frontend` módulo. Ele incluirá o arquivo `_variables.scss`, que contém uma série de variáveis de marca a serem usadas em diferentes arquivos Sass no projeto. O arquivo `_base.scss` também está incluído e define alguns estilos básicos para elementos HTML. Uma expressão regular inclui todos os estilos para estilos de componentes individuais em `src/main/webpack/components`. Outra expressão regular inclui todos os arquivos em `src/main/webpack/site/styles`.

1. Inspect o arquivo `main.ts`. `main.ts` inclui  `main.scss` e inclui uma expressão regular para coletar qualquer  `.js` ou  `.ts` arquivos no projeto. Este ponto de entrada será usado pelos [arquivos de configuração do webpack](https://webpack.js.org/configuration/) como o ponto de entrada para todo o módulo `ui.frontend`.

1. Inspect os arquivos abaixo de `src/main/webpack/site/styles`:

   ![Arquivos de estilo](assets/client-side-libraries/style-files.png)

   Esses arquivos usam estilos para elementos globais no modelo, como Cabeçalho, Rodapé e container de conteúdo principal. As regras CSS nesses arquivos público alvo elementos HTML diferentes `header`, `main` e `footer`. Esses elementos HTML foram definidos por políticas no capítulo anterior [Páginas e Modelos](./pages-templates.md).

1. Expanda a pasta `components` em `src/main/webpack` e inspecione os arquivos.

   ![Arquivos do Component Sass](assets/client-side-libraries/component-sass-files.png)

   Cada arquivo mapeia para um Componente principal como o [Componente Acordeão](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Cada componente principal é construído com a notação [Block Element Modifier](https://getbem.com/) ou BEM para facilitar o público alvo de classes CSS específicas por regras de estilo. Os arquivos abaixo de `/components` foram capturados pelo AEM Project Archetype com as diferentes regras BEM para cada componente.

1. Baixe os estilos base WKND **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-src.zip)** e **unzip** o arquivo.

   ![Estilos de base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar o tutorial, fornecemos os vários arquivos Sass que implementam a marca WKND com base nos Componentes principais e na estrutura do Modelo de página de artigo.

1. Substitua o conteúdo de `ui.frontend/src` por arquivos da etapa anterior. O conteúdo do zip deve substituir as seguintes pastas:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Arquivos alterados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect os arquivos alterados para ver detalhes da implementação do estilo WKND.

## Inspect a integração ui.frontenda {#ui-frontend-integration}

Uma peça chave de integração incorporada ao módulo **ui.frontende**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) obtém os artefatos CSS e JS compilados de um projeto webpack/npm e os transforma em AEM bibliotecas do lado do cliente.

![integração da arquitetura ui.frontender](assets/client-side-libraries/ui-frontend-architecture.png)

O AEM Project Archetype configura automaticamente essa integração. Em seguida, explore como funciona.


1. Abra um terminal de linha de comando e instale o módulo **ui.frontende** usando o comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` só precisa ser executado uma vez, após um novo clone ou geração do projeto.

1. No mesmo terminal, crie e implante o módulo **ui.frontende** usando o comando `npm run dev`:

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Você pode receber um erro como &quot;ERROR in ./src/main/webpack/site/main.scss&quot;.
   > Isso normalmente acontece porque seu ambiente mudou desde a execução de `npm install`.
   > Execute `npm rebuild node-sass` para corrigir o problema. Isso ocorrerá se a versão de `npm` instalada em sua máquina dev local for diferente da versão usada pelo Maven `frontend-maven-plugin` no arquivo `aem-guides-wknd/pom.xml`. Você pode corrigir isso permanentemente modificando a versão no arquivo pom para corresponder à sua versão local ou vice-versa.

1. O comando `npm run dev` deve criar e compilar o código fonte do projeto do Webpack e, em última análise, preencher os **clientlib-site** e **clientlib-dependencies** no módulo **ui.apps**.

   >[!NOTE]
   >
   >Também há um perfil `npm run prod` que minimizará o JS e o CSS. Esta é a compilação padrão sempre que a compilação do webpack é acionada via Maven. Mais detalhes sobre o módulo [ui.frontenda podem ser encontrados aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect o arquivo `site.css` abaixo de `ui.frontend/dist/clientlib-site/css/site.css`. Este é o CSS compilado com base nos arquivos de origem do Sass.

   ![Css do site distribuído](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect o arquivo `ui.frontend/clientlib.config.js`. Este é o arquivo de configuração para um plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma o conteúdo de `/dist` em uma biblioteca de cliente e o move para o módulo `ui.apps`.

1. Inspect o arquivo `site.css` no módulo **ui.apps** em `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Essa deve ser uma cópia idêntica do arquivo `site.css` do módulo **ui.frontendent**. Agora que está no módulo **ui.apps**, ele pode ser implantado no AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Como **clientlib-site** é compilado durante o tempo de compilação, usando **npm** ou **maven**, ele pode ser ignorado com segurança do controle de origem no módulo **ui.apps**. Inspect o arquivo `.gitignore` abaixo de **ui.apps**.

1. Sincronize a biblioteca `clientlib-site` com uma instância local de AEM usando as ferramentas do desenvolvedor ou as habilidades Maven.

   ![Sincronizar Site Clientlib](assets/client-side-libraries/sync-clientlib-site.png)

1. Abra o artigo de LA Skatepark em AEM em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos básicos atualizados para o artigo](assets/client-side-libraries/updated-base-styles.png)

   Agora você deve ver os estilos atualizados do artigo. Talvez seja necessário fazer uma atualização para limpar os arquivos CSS armazenados em cache pelo navegador.

   Está começando a parecer muito mais perto dos modelos!

   >[!NOTE]
   >
   > As etapas executadas acima para criar e implantar o código ui.frontenda no AEM serão executadas automaticamente quando uma compilação Maven for acionada a partir da raiz do projeto `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> O uso do módulo **ui.frontendent** pode não ser necessário para todos os projetos. O módulo **ui.front-end** adiciona complexidade adicional e, se não houver necessidade/desejo de usar algumas dessas ferramentas avançadas de front-end (Sass, webpack, npm...), ele pode não ser necessário.

## Inclusão de Página e Modelo {#page-inclusion}

Em seguida, vamos rever como os clientlibs são referenciados na Página AEM. Uma prática recomendada comum no desenvolvimento da Web é incluir o CSS no cabeçalho HTML `<head>` e no JavaScript antes de fechar a tag `</body>`.

1. No módulo **ui.apps**, navegue até `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Componente da Página de Estrutura](assets/client-side-libraries/customheaderlibs-html.png)

   Este é o componente `page` usado para renderizar todas as páginas na implementação do WKND.

1. Abra o arquivo `customheaderlibs.html`. Observe as linhas `${clientlib.css @ categories='wknd.base'}`. Isso indica que o CSS para a clientlib com uma categoria de `wknd.base` será incluído por meio desse arquivo, incluindo efetivamente **clientlib-base** no cabeçalho de todas as páginas.

1. Atualize `customheaderlibs.html` para incluir uma referência aos estilos de fonte do Google que especificamos anteriormente no módulo **ui.frontendent**.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect o arquivo `customfooterlibs.html`. Esse arquivo, como `customheaderlibs.html`, deve ser substituído pela implementação de projetos. Aqui a linha `${clientlib.js @ categories='wknd.base'}` significa que o JavaScript de **clientlib-base** será incluído na parte inferior de todas as nossas páginas.

1. Exporte o componente `page` para o servidor AEM usando as ferramentas do desenvolvedor ou suas habilidades Maven.

1. Navegue até o modelo Página de artigo em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Clique no ícone **Informações da página** e, no menu, selecione **Política de página** para abrir a caixa de diálogo **Política de página**.

   ![Política de Página do Menu de Modelo de Página do Artigo](assets/client-side-libraries/template-page-policy.png)

   *Informações da página > Política da página*

1. Observe que as categorias para `wknd.dependencies` e `wknd.site` estão listadas aqui. Por padrão, os clientlibs configurados por meio da Política de página são divididos para incluir o CSS no cabeçalho da página e o JavaScript no final do corpo. Se desejar, você pode lista explicitamente que o clientlib JavaScript seja carregado no cabeçalho da Página. É o caso de `wknd.dependencies`.

   ![Política de Página do Menu de Modelo de Página do Artigo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Também é possível fazer referência ao `wknd.site` ou `wknd.dependencies` do componente de página diretamente, usando o script `customheaderlibs.html` ou `customfooterlibs.html`, como vimos anteriormente para a clientlib `wknd.base`. O uso do modelo oferece alguma flexibilidade para que você possa escolher quais clientlibs são usados por modelo. Por exemplo, se você tiver uma biblioteca JavaScript muito pesada que só será usada em um modelo selecionado.

1. Navegue até a página **LA Skatepark** criada usando o **Modelo de página do artigo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Você deve ver uma diferença nas fontes.

1. Clique no ícone **Informações da página** e, no menu, selecione **Visualização como publicada** para abrir a página do artigo fora do editor de AEM.

   ![Exibir como publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualização a fonte de página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e você deve ser capaz de ver as seguintes referências clientlib na `<head>`:

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que os clientlibs estão usando o terminal proxy `/etc.clientlibs`. Você também deve ver a seguinte clientlib incluída na parte inferior da página:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >É importante no lado da publicação que as bibliotecas do cliente sejam **not** servidas de **/apps**, pois esse caminho deve ser restrito por motivos de segurança usando a seção de filtro [Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). A [propriedade allowProxy](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/introduction/clientlibs.translate.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) da biblioteca do cliente garante que o CSS e o JS sejam fornecidos de **/etc.clientlibs**.

## Webpack DevServer - Marcação Estática {#webpack-dev-static}

Nos exercícios anteriores, pudemos atualizar vários arquivos Sass no módulo **ui.frontendent** e, por meio de um processo de criação, ver essas alterações refletidas no AEM. Em seguida, vamos observar uma técnica que aproveita um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desenvolver rapidamente nossos estilos de front-end em **static** HTML.

Essa técnica é útil se a maioria dos estilos e do código front-end for executada por um desenvolvedor Front-End dedicado que pode não ter acesso fácil a um ambiente AEM. Essa técnica também permite que o FED faça modificações diretamente no HTML, que pode ser entregue a um desenvolvedor AEM para implementá-lo como componentes.

1. Copie a fonte da página da página de artigo do skatepark LA em [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Reabra seu IDE. Cole a marcação copiada do AEM para `index.html` no módulo **ui.frontende** abaixo de `src/main/webpack/static`.
1. Edite a marcação copiada e remova quaisquer referências a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Podemos remover essas referências porque o servidor dev de webpack gerará esses artefatos automaticamente.

1. Start o servidor de desenvolvimento de webpack de um novo terminal executando o seguinte comando no módulo **ui.front-end**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Isso deve abrir uma nova janela do navegador em [http://localhost:8080/](http://localhost:8080/) com marcação estática.

1. Edite o arquivo `src/main/webpack/site/_variables.scss`. Substitua a regra `$text-color` pelo seguinte:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salve as alterações.

1. Você deve ver automaticamente as alterações refletidas no navegador em [http://localhost:8080](http://localhost:8080).

   ![Alterações no servidor de desenvolvimento de webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise o arquivo `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contém a configuração do webpack usada para start do servidor webpack-dev. Observe que ele faz proxy dos caminhos `/content` e `/etc.clientlibs` de uma instância de AEM em execução local. É assim que as imagens e outros clientlibs (não gerenciados pelo código **ui.frontendent**) são disponibilizados.

   >[!CAUTION]
   >
   > A imagem src da marcação estática aponta para um componente de imagem ao vivo em uma instância de AEM local. As imagens aparecerão quebradas se o caminho para a imagem for alterado, se AEM não for iniciado ou se o navegador não tiver feito logon na instância AEM local. Se estiver distribuindo para um recurso externo, também será possível substituir as imagens por referências estáticas.

1. Você pode **parar** o servidor do webpack da linha de comando digitando `CTRL+C`.

## Webpack DevServer - Assista e sincronize {#webpack-dev-watch}

Outra técnica é fazer com que o Node.js observe quaisquer alterações de arquivo nos arquivos src no módulo `ui.frontend`. Sempre que um arquivo for alterado, ele compilará rapidamente a biblioteca do cliente e usará o módulo [aemsync](https://www.npmjs.com/package/aemsync) npm para sincronizar as alterações em um servidor AEM em execução.

1. Start o servidor dev de webpack no modo **watch** de um novo terminal executando o seguinte comando no módulo **ui.frontendent**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Isso compilará os arquivos `src` e sincronizará as alterações com AEM em [http://localhost:4502](http://localhost:4502)

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. Navegue até AEM e o artigo LA Skatepark: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Alterações implantadas em AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   As alterações devem ser implantadas em AEM. Há um pequeno atraso e você precisará atualizar o navegador manualmente para ver as atualizações. No entanto, visualizar as alterações diretamente no AEM é benéfico se você estiver trabalhando com novos componentes e criação de diálogo.

1. Reverta a alteração para `_variables.scss` e salve as alterações. As alterações devem ser sincronizadas novamente com a instância local do AEM após um pequeno atraso.

1. Pare o servidor de desenvolvimento de webpack e execute uma compilação Maven completa a partir da raiz do projeto:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Novamente, o módulo `ui.frontend` é compilado, transformado em bibliotecas de clientes e implantado em AEM pelo módulo `ui.apps`. Mas desta vez Maven faz tudo por nós.

## Parabéns! {#congratulations}

Parabéns, a Página de artigo agora tem alguns estilos consistentes que correspondem à marca WKND e você se familiarizou com o módulo **ui.frontendent**!

### Próximas etapas {#next-steps}

Saiba mais sobre como implementar estilos individuais e usar os Componentes principais usando o Sistema de estilo Experience Manager. [Desenvolver com o ](style-system.md) Sistema de estilo abrange o uso do Sistema de estilo para estender os Componentes principais com CSS específico da marca e configurações avançadas de política do Editor de modelos.

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `tutorial/client-side-libraries-solution`.

1. Clique no repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/client-side-libraries-solution`.

## Ferramentas e recursos adicionais {#additional-resources}

### aemfeed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis é uma ferramenta de linha de comando open-source que pode ser usada para acelerar o desenvolvimento front-end. Ele é acionado por [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Em um nível alto, **aemfeed** foi projetado para acompanhar as alterações de arquivos no módulo **ui.apps** e sincronizá-las automaticamente diretamente em uma instância AEM em execução. Com base nas alterações, um navegador local será atualizado automaticamente, acelerando assim o desenvolvimento de front-end. Ele também foi desenvolvido para trabalhar com o Sling Log tracer para exibir automaticamente quaisquer erros do lado do servidor diretamente no terminal.

Se você estiver trabalhando muito no módulo **ui.apps**, modificando scripts HTL e criando componentes personalizados, **aemfeed** pode ser uma ferramenta muito poderosa para usar. [A documentação completa pode ser encontrada aqui.](https://github.com/abmaonline/aemfed).

### Depuração de bibliotecas do cliente {#debugging-clientlibs}

Com diferentes métodos de **categoria** e **incorpora** para incluir várias bibliotecas de clientes, pode ser difícil solucionar problemas. AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é **Recriar bibliotecas de clientes**, que forçará AEM a recompilar quaisquer arquivos MENOS e a gerar o CSS.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - Lista todas as bibliotecas clientes registradas na instância AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Saída**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  de teste - permite que um usuário veja a saída HTML esperada de clientlib inclui com base na categoria.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validação**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  de dependências de bibliotecas - realça quaisquer dependências ou categorias incorporadas que não possam ser encontradas.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Recriar bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  de clientes - permite que um usuário force AEM reconstruir todas as bibliotecas de clientes ou invalidar o cache das bibliotecas de clientes. Essa ferramenta é particularmente eficaz ao desenvolver com MENOS, pois pode forçar AEM a recompilar o CSS gerado. Em geral, é mais eficaz invalidar caches e, em seguida, executar uma atualização de página em vez de recriar todas as bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruir biblioteca do cliente](assets/client-side-libraries/rebuild-clientlibs.png)
