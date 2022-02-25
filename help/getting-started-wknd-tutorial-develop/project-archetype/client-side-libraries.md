---
title: Bibliotecas do lado do cliente e fluxo de trabalho de front-end
description: Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar CSS e Javascript para uma implementação de Sites da Adobe Experience Manager (AEM). Este tutorial também abordará como o módulo ui.frontend, um projeto de webpack, pode ser integrado ao processo de criação completo.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '2889'
ht-degree: 1%

---

# Bibliotecas do lado do cliente e fluxo de trabalho de front-end {#client-side-libraries}

Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar CSS e Javascript para uma implementação de Sites da Adobe Experience Manager (AEM). Este tutorial também abordará como a [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) módulo, um desacoplamento [webpack](https://webpack.js.org/) projeto pode ser integrado ao processo de criação completo.

## Pré-requisitos {#prerequisites}

Revise as ferramentas necessárias e as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

Também é recomendável revisar a [Noções básicas sobre componentes](component-basics.md#client-side-libraries) tutorial para entender os fundamentos das bibliotecas e AEM do lado do cliente.

### Projeto inicial

>[!NOTE]
>
> Se você concluiu o capítulo anterior com êxito, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Confira o código base que o tutorial constrói em:

1. Confira o `tutorial/client-side-libraries-start` ramificação de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implante a base de código em uma instância de AEM local usando suas habilidades Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe a `classic` para qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou verifique o código localmente, alternando para a ramificação `tutorial/client-side-libraries-solution`.

## Objetivos

1. Entenda como as bibliotecas do lado do cliente são incluídas em uma página por meio de um modelo editável.
1. Saiba como usar o Módulo UI.Frontend e um servidor de desenvolvimento de webpack para desenvolvimento front-end dedicado.
1. Entenda o fluxo de trabalho completo do fornecimento de CSS e JavaScript compilados para uma implementação do Sites.

## O que você vai criar {#what-you-will-build}

Neste capítulo, você adicionará alguns estilos de linha de base para o site WKND e o Modelo da página de artigo em um esforço para aproximar a implementação do [Modelos de design da interface do usuário](assets/pages-templates/wknd-article-design.xd). Você usará um fluxo de trabalho avançado de front-end para integrar um projeto de webpack a uma biblioteca AEM cliente.

![Estilos Concluídos](assets/client-side-libraries/finished-styles.png)

*Página do artigo com estilos de linha de base aplicados*

## Segundo plano {#background}

As bibliotecas do lado do cliente fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As metas básicas das bibliotecas do lado do cliente ou clientlibs são:

1. Armazene CSS/JS em pequenos arquivos discretos para facilitar o desenvolvimento e a manutenção
1. Gerencie dependências em estruturas de terceiros de forma organizada
1. Minimize o número de solicitações do lado do cliente, concatenando CSS/JS em uma ou duas solicitações.

Mais informações sobre o uso [Bibliotecas do lado do cliente podem ser encontradas aqui.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

As bibliotecas do lado do cliente têm algumas limitações. O mais notável é um suporte limitado para idiomas de front-end populares como Sass, LESS e TypeScript. No tutorial, observaremos como a variável **ui.frontend** pode ajudar a resolver isso.

Implante a base de código inicial em uma instância de AEM local e navegue até [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). No momento, essa página não tem estilo. Em seguida, implementaremos bibliotecas do lado do cliente para a marca WKND para adicionar CSS e Javascript à página.

## Organização de bibliotecas do lado do cliente {#organization}

Em seguida, exploraremos a organização de clientlibs geradas pelo [Arquétipo de projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![Organização de bibliotecas de clientes de alto nível](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nível Organização da biblioteca do lado do cliente e inclusão da página*

>[!NOTE]
>
> A seguinte organização de biblioteca do lado do cliente é gerada AEM Arquétipo de projeto, mas representa apenas um ponto de partida. Como um projeto gerencia e fornece CSS e Javascript para uma implementação de Sites pode variar consideravelmente com base em recursos, conjuntos de habilidades e requisitos.

1. Usando o VSCode ou outro IDE, abra o **ui.apps** módulo.
1. Expandir o caminho `/apps/wknd/clientlibs` para exibir as clientlibs geradas pelo arquétipo.

   ![Clientlibs em ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspecionaremos essas clientlibs com mais detalhes abaixo.

1. A tabela a seguir resume as bibliotecas de clientes. Mais detalhes sobre [incluindo bibliotecas de clientes, pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrição | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nível básico de CSS e JavaScript necessários para o funcionamento do Site WKND | incorporar bibliotecas do cliente do Componente principal |
   | `clientlib-grid` | Gera o CSS necessário para [Modo de layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) para trabalhar. | Os pontos de interrupção para dispositivos móveis/tablets podem ser configurados aqui |
   | `clientlib-site` | Contém tema específico do site para o site WKND | Gerado pelo `ui.frontend` módulo |
   | `clientlib-dependencies` | Incorpora quaisquer dependências de terceiros | Gerado pelo `ui.frontend` módulo |

1. Observe que `clientlib-site` e `clientlib-dependencies` são ignoradas do controle de origem. Isso é por design, já que eles serão gerados no momento da criação pela variável `ui.frontend` módulo.

## Atualizar estilos base {#base-styles}

Em seguida, atualize os estilos base definidos na variável **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** módulo. Os arquivos na `ui.frontend` o módulo gerará `clientlib-site` e `clientlib-dependecies` bibliotecas que contêm o tema do Site e quaisquer dependências de terceiros.

As bibliotecas do lado do cliente têm algumas limitações quando se trata de suporte a idiomas como [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Há várias ferramentas de código aberto como [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) que aceleram e otimizam o desenvolvimento de front-end. O objetivo da **ui.frontend** O módulo do é ser capaz de usar essas ferramentas para gerenciar a maioria dos arquivos de origem front-end.

1. Abra o **ui.frontend** e navegue até `src/main/webpack/site`.
1. Abra o arquivo `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)
bibliotecas do lado do cliente/main-scss

   `main.scss` é o ponto de entrada para todos os arquivos Sass no `ui.frontend` módulo. Ele incluirá a variável `_variables.scss` , que contém uma série de variáveis de marca a serem usadas em diferentes arquivos Sass no projeto. O `_base.scss` também é incluído e define alguns estilos básicos para elementos HTML. Uma expressão regular inclui todos os estilos de componentes individuais em `src/main/webpack/components`. Outra expressão regular inclui todos os arquivos em `src/main/webpack/site/styles`.

1. Inspect o arquivo `main.ts`. Inclui `main.scss` e uma expressão regular para coletar qualquer `.js` ou `.ts` arquivos no projeto. Esse ponto de entrada será usado pela variável [arquivos de configuração do webpack](https://webpack.js.org/configuration/) como ponto de entrada para a totalidade `ui.frontend` módulo.

1. Inspect os arquivos abaixo `src/main/webpack/site/styles`:

   ![Arquivos de estilo](assets/client-side-libraries/style-files.png)

   Esses arquivos possuem estilos para elementos globais no modelo, como Cabeçalho, Rodapé e contêiner de conteúdo principal. As regras de CSS nesses arquivos têm como alvo diferentes elementos de HTML `header`, `main`e  `footer`. Esses elementos de HTML foram definidos por políticas no capítulo anterior [Páginas e modelos](./pages-templates.md).

1. Expanda o `components` pasta em `src/main/webpack` e inspecione os arquivos.

   ![Arquivos Sass de Componentes](assets/client-side-libraries/component-sass-files.png)

   Cada arquivo mapeia para um Componente principal como o [Componente acordeão](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Cada Componente principal é criado com [Modificador de elemento de bloco](https://getbem.com/) ou notação BEM para facilitar o direcionamento de classes CSS específicas com regras de estilo. Os arquivos abaixo `/components` Foram criadas pelo Arquétipo de projeto AEM com as diferentes regras BEM para cada componente.

1. Baixe os estilos base WKND **[wknd-base-styles-src-v3.zip]/(help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** e **descompactar** o arquivo .

   ![Estilos Base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar o tutorial, fornecemos os vários arquivos Sass que implementam a marca WKND com base nos Componentes principais e na estrutura do Modelo de página de artigo.

1. Substituir o conteúdo de `ui.frontend/src` com arquivos da etapa anterior. O conteúdo do zip deve substituir as seguintes pastas:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Arquivos alterados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect os arquivos alterados para ver detalhes da implementação do estilo WKND.

## Integração do Inspect com o ui.frontend {#ui-frontend-integration}

Um elemento-chave de integração integrado no **ui.frontend** módulo, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) O pega os artefatos CSS e JS compilados de um projeto do webpack/npm e os transforma em bibliotecas AEM do lado do cliente.

![integração da arquitetura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

O Arquétipo de projeto AEM configura essa integração automaticamente. Em seguida, explore como funciona.


1. Abra um terminal de linha de comando e instale o **ui.frontend** usando o `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` só precisa ser executado uma vez, após um novo clone ou geração do projeto.

1. Inicie o servidor de desenvolvimento do webpack em **assistir** executando o seguinte comando:

   ```shell
   $ npm run watch
   ```

1. Isso compilará a variável `src` nos arquivos da `ui.frontend` e sincronize as alterações com o AEM em [http://localhost:4502](http://localhost:4502)

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

1. O comando `npm run watch` preenche a variável **clientlib-site** e **clientlib-dependencies** no **ui.apps** em seguida, é sincronizado automaticamente com o AEM.

   >[!NOTE]
   >
   >Há também um `npm run prod` perfil que minimizará o JS e o CSS. Esta é a compilação padrão sempre que a build do webpack é acionada via Maven. Mais detalhes sobre o [O módulo ui.frontend pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect o arquivo `site.css` debaixo `ui.frontend/dist/clientlib-site/site.css`. Este é o CSS compilado com base nos arquivos de origem do Sass.

   ![Css do site distribuído](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect o arquivo `ui.frontend/clientlib.config.js`. Este é o arquivo de configuração de um plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma o conteúdo de `/dist` em uma biblioteca do cliente e a move para a `ui.apps` módulo.

1. Inspect o arquivo `site.css` no **ui.apps** módulo em `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Essa deve ser uma cópia idêntica da variável `site.css` do **ui.frontend** módulo. Agora que está **ui.apps** pode ser implantado no AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Since **clientlib-site** é compilado durante o tempo de criação, usando **npm** ou **maven**, pode ser ignorado com segurança do controle de origem no **ui.apps** módulo. A Inspect `.gitignore` arquivo debaixo de **ui.apps**.

1. Abra o artigo do LA Skatepark em AEM em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos de base atualizados para o artigo](assets/client-side-libraries/updated-base-styles.png)

   Agora você deve ver os estilos atualizados do artigo. Talvez seja necessário fazer uma atualização rígida para limpar todos os arquivos CSS armazenados em cache pelo navegador.

   Está começando a parecer muito mais perto dos modelos!

   >[!NOTE]
   >
   > As etapas executadas acima para criar e implantar o código ui.frontend em AEM serão executadas automaticamente quando uma build Maven for acionada da raiz do projeto `mvn clean install -PautoInstallSinglePackage`.

## Fazer uma alteração de estilo

Em seguida, faça uma pequena alteração no `ui.frontend` para ver o `npm run watch` implante os estilos automaticamente na instância de AEM local.

1. No `ui.frontend` o módulo abre o arquivo : `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Atualize o `$brand-primary` variável de cor:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Salve as alterações.

1. Retorne ao navegador e atualize a página de AEM para ver as atualizações:

   ![Bibliotecas do lado do cliente](assets/client-side-libraries/style-update-brand-primary.png)

1. Reverter a alteração para a `$brand-primary` colorir e parar a criação do webpack usando o comando `CTRL+C`.

>[!CAUTION]
>
> O uso da variável **ui.frontend** pode não ser necessário para todos os projetos. O **ui.frontend** O módulo adiciona complexidade adicional e, se não houver necessidade/desejo de usar algumas dessas ferramentas front-end avançadas (Sass, webpack, npm..), talvez não seja necessário.

## Inclusão de página e modelo {#page-inclusion}

Em seguida, vamos analisar como as clientlibs são referenciadas na Página AEM. Uma prática recomendada comum no desenvolvimento da Web é incluir o CSS no Cabeçalho do HTML `<head>` e JavaScript antes de fechar `</body>` .

1. Navegue até o modelo Página de artigo em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Clique no botão **Informações da página** e, no menu, selecione **Política da página** para abrir o **Política da página** caixa de diálogo.

   ![Política da página do menu Modelo de página do artigo](assets/client-side-libraries/template-page-policy.png)

   *Informações da página > Política da página*

1. Observe que as categorias de `wknd.dependencies` e `wknd.site` estão listadas aqui. Por padrão, as clientlibs configuradas por meio da Política de página são divididas para incluir o CSS no cabeçalho da página e o JavaScript no final do corpo. Se desejar, é possível listar explicitamente que o JavaScript clientlib deve ser carregado no Cabeçalho da página. É o caso para `wknd.dependencies`.

   ![Política da página do menu Modelo de página do artigo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Também é possível fazer referência à variável `wknd.site` ou `wknd.dependencies` diretamente do componente da página, usando o `customheaderlibs.html` ou `customfooterlibs.html` , como vimos anteriormente para a variável `wknd.base` clientlib. Usar o modelo oferece alguma flexibilidade, permitindo que você escolha quais clientlibs serão usados por modelo. Por exemplo, se você tiver uma biblioteca JavaScript muito pesada que só será usada em um modelo selecionado.

1. Navegue até o **LA Skateparks** página criada usando o **Modelo da página do artigo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Clique no botão **Informações da página** e, no menu, selecione **Exibir como publicado** para abrir a página do artigo fora do editor de AEM.

   ![Exibir como publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Exibir a origem da página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e você deve ser capaz de ver as seguintes referências de clientlib na variável `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que as clientlibs estão usando o proxy `/etc.clientlibs` endpoint . Você também deve ver a seguinte clientlib inclui na parte inferior da página:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Se a seguir no 6.5/6.4, as bibliotecas do lado do cliente não serão minificadas automaticamente. Consulte a documentação no [Gerenciador da biblioteca de HTML para ativar a minificação (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >É essencial no lado da publicação que as bibliotecas do cliente sejam **not** servido de **/apps** já que esse caminho deve ser restrito por motivos de segurança usando a variável [Seção de filtro do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). O [propriedade allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) da biblioteca do cliente garante que o CSS e o JS sejam fornecidos **/etc.clientlibs**.

### Próximas etapas {#next-steps}

Saiba como implementar estilos individuais e usar os Componentes principais usando o Sistema de estilo Experience Manager. [Desenvolvimento com o sistema de estilos](style-system.md) O abrange o uso do Sistema de estilos para estender os Componentes principais com CSS específico da marca e configurações avançadas de política do Editor de modelos.

Exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na chave Git `tutorial/client-side-libraries-solution`.

1. Clonar o [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositório.
1. Confira o `tutorial/client-side-libraries-solution` ramificação.

## Ferramentas e recursos adicionais {#additional-resources}

### WebPack DevServer - Marcação estática {#webpack-dev-static}

Nos exercícios anteriores, pudemos atualizar vários arquivos Sass na **ui.frontend** e por meio de um processo de criação, veja essas alterações refletidas em AEM. Em seguida, observaremos uma técnica que utiliza um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desenvolver rapidamente nossos estilos de front-end contra **estático** HTML.

Essa técnica é útil se a maioria dos estilos e do código front-end for executada por um desenvolvedor front-end dedicado que pode não ter acesso fácil a um ambiente AEM. Essa técnica também permite que o FED faça modificações diretamente no HTML, que pode ser entregue a um desenvolvedor de AEM para implementar como componentes.

1. Copie a fonte da página da página de artigo do skatepark de LA em [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Abra novamente o IDE. Cole a marcação copiada do AEM no `index.html` no **ui.frontend** módulo abaixo `src/main/webpack/static`.
1. Edite a marcação copiada e remova quaisquer referências ao **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Podemos remover essas referências porque o servidor de desenvolvimento do webpack gerará esses artefatos automaticamente.

1. Inicie o servidor de desenvolvimento do webpack a partir de um novo terminal executando o seguinte comando a partir do **ui.frontend** módulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Isso deve abrir uma nova janela do navegador em [http://localhost:8080/](http://localhost:8080/) com marcação estática.

1. Editar o arquivo `src/main/webpack/site/_variables.scss` arquivo. Substitua o `$text-color` regra com o seguinte:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salve as alterações.

1. Você deve ver as alterações automaticamente refletidas no navegador em [http://localhost:8080](http://localhost:8080).

   ![Alterações no servidor de desenvolvimento do webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise o `/aem-guides-wknd.ui.frontend/webpack.dev.js` arquivo. Contém a configuração do webpack usada para iniciar o webpack-dev-server. Observe que ele faz proxy nos caminhos `/content` e `/etc.clientlibs` de uma instância de AEM em execução localmente. As imagens e outras clientlibs são assim (não gerenciadas pela variável **ui.frontend** ).

   >[!CAUTION]
   >
   > A imagem src da marcação estática aponta para um componente de imagem ao vivo em uma instância de AEM local. As imagens aparecerão quebradas se o caminho para a imagem for alterado, se AEM não for iniciado ou se o navegador não tiver feito logon na instância de AEM local. Ao entregar para um recurso externo, também é possível substituir as imagens por referências estáticas.

1. Você pode **stop** o servidor webpack da linha de comando digitando `CTRL+C`.

### amamentado {#develop-aemfed}

[**amamentado**](https://aemfed.io/) é uma ferramenta de linha de comando e código aberto que pode ser usada para acelerar o desenvolvimento front-end. É alimentado por  [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) e [Rastreador de log do Sling](https://sling.apache.org/documentation/bundles/log-tracers.html).

Em um alto nível **amamentado** O foi projetado para acompanhar alterações de arquivo no **ui.apps** e sincroniza-as automaticamente diretamente em uma instância AEM em execução. Com base nas alterações, um navegador local será atualizado automaticamente, acelerando assim o desenvolvimento front-end. Ele também foi criado para funcionar com o rastreador do Sling Log para exibir automaticamente quaisquer erros do lado do servidor diretamente no terminal.

Se você estiver fazendo muito trabalho dentro do **ui.apps** módulo, modificação de scripts HTL e criação de componentes personalizados, **amamentado** pode ser uma ferramenta muito poderosa para usar. [A documentação completa pode ser encontrada aqui.](https://github.com/abmaonline/aemfed).

### Depuração de bibliotecas do lado do cliente {#debugging-clientlibs}

Com diferentes métodos de **categorias** e **incorporar** para incluir várias bibliotecas de clientes, pode ser difícil solucionar problemas. AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é **Recriar bibliotecas de clientes** que forçará o AEM a recompilar quaisquer arquivos MENOS e gerar o CSS.

* [**Despejar dedos**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista todas as bibliotecas de clientes registradas na instância de AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Saída de teste**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permite que um usuário veja a saída HTML esperada das inclusões de clientlib com base na categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validação de dependências de bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - destaca as dependências ou categorias incorporadas que não podem ser encontradas. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Recriar bibliotecas de clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permite que um usuário force o AEM a reconstruir todas as bibliotecas do cliente ou invalidar o cache das bibliotecas do cliente. Essa ferramenta é particularmente eficaz ao desenvolver com MENOS, pois pode forçar o AEM a recompilar o CSS gerado. Em geral, é mais eficaz Invalidar caches e depois executar uma atualização de página em vez de reconstruir todas as bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruir biblioteca do cliente](assets/client-side-libraries/rebuild-clientlibs.png)
