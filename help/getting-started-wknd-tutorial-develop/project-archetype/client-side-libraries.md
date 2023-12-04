---
title: Bibliotecas de clientes e fluxo de trabalho de front-end
description: Saiba como usar bibliotecas de clientes para implantar e gerenciar CSS e JavaScript para uma implementação do Adobe Experience Manager (AEM) Sites. Saiba como o módulo ui.frontend, um projeto de webpack, pode ser integrado ao processo de build completo.
version: 6.4, 6.5, Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 752
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 0%

---

# Bibliotecas de clientes e fluxo de trabalho de front-end {#client-side-libraries}

Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar o CSS e o JavaScript para uma implementação do Adobe Experience Manager (AEM) Sites. Este tutorial também aborda como as [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) um módulo dissociado [webpack](https://webpack.js.org/) projeto, podem ser integrados ao processo de build completo.

## Pré-requisitos {#prerequisites}

Analisar as ferramentas e instruções necessárias para a configuração de um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

É também recomendável rever a [Noções básicas sobre componentes](component-basics.md#client-side-libraries) tutorial para entender os fundamentos das bibliotecas do lado do cliente e do AEM.

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira o `tutorial/client-side-libraries-start` ramificar de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implante a base de código em uma instância de AEM local usando suas habilidades de Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o `classic` para qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou confira o código localmente alternando para a ramificação `tutorial/client-side-libraries-solution`.

## Objetivos

1. Entenda como as bibliotecas do lado do cliente são incluídas em uma página por meio de um modelo editável.
1. Saiba como usar o `ui.frontend` e um servidor de desenvolvimento de webpack para desenvolvimento front-end dedicado.
1. Entenda o fluxo de trabalho completo de entrega de CSS e JavaScript compilados para uma implementação do Sites.

## O que você vai criar {#what-build}

Neste capítulo, você adiciona alguns estilos de linha de base para o site da WKND e o Modelo de página de artigo para aproximar a implementação da [Modelos de design da interface do usuário](assets/pages-templates/wknd-article-design.xd). Você usa um fluxo de trabalho avançado de front-end para integrar um projeto de webpack em uma biblioteca de cliente AEM.

![Estilos concluídos](assets/client-side-libraries/finished-styles.png)

*Página de artigo com estilos de linha de base aplicados*

## Segundo plano {#background}

As bibliotecas do lado do cliente fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As metas básicas para bibliotecas do lado do cliente ou clientlibs são:

1. Armazene CSS/JS em arquivos pequenos separados para facilitar o desenvolvimento e a manutenção
1. Gerenciar dependências em estruturas de terceiros de maneira organizada
1. Minimize o número de solicitações do lado do cliente concatenando CSS/JS em uma ou duas solicitações.

Mais informações sobre como usar o [As bibliotecas do lado do cliente podem ser encontradas aqui.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

As bibliotecas do lado do cliente têm algumas limitações. Mais notavelmente, é um suporte limitado para linguagens de front-end populares como Sass, LESS e TypeScript. No tutorial, vamos examinar como a variável **ui.frontend** pode ajudar a resolver isso.

Implante a base de código inicial em uma instância de AEM local e navegue até [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página não tem estilo. Vamos implementar bibliotecas do lado do cliente para a marca WKND para adicionar CSS e JavaScript à página.

## Organização de bibliotecas do lado do cliente {#organization}

A seguir, vamos explorar a organização das bibliotecas de clientes geradas pelo [Arquétipo de projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR).

![Organização de alto nível da biblioteca do cliente](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Organização da biblioteca do lado do cliente e inclusão da página no diagrama de alto nível*

>[!NOTE]
>
> A seguinte organização de biblioteca do lado do cliente é gerada pelo Arquétipo de projeto AEM, mas representa apenas um ponto de partida. A forma como um projeto gerencia e fornece CSS e JavaScript para uma implementação do Sites pode variar bastante com base em recursos, conjuntos de habilidades e requisitos.

1. Usando o VSCode ou outro IDE, abra o **ui.apps** módulo.
1. Expanda o caminho `/apps/wknd/clientlibs` para exibir as clientlibs geradas pelo arquétipo.

   ![Clientlibs em ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Na seção abaixo, essas clientlibs são analisadas com mais detalhes.

1. A tabela a seguir resume as bibliotecas de clientes. Mais detalhes sobre [incluindo as Bibliotecas de clientes podem ser encontradas aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nome | Descrição | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nível base de CSS e JavaScript necessários para o funcionamento do Site WKND | incorpora bibliotecas de cliente dos Componentes principais |
   | `clientlib-grid` | Gera o CSS necessário para [Modo de layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) para trabalhar. | Os pontos de interrupção de dispositivo móvel/tablet podem ser configurados aqui |
   | `clientlib-site` | Contém um tema específico do site para o site WKND | Gerado pelo `ui.frontend` módulo |
   | `clientlib-dependencies` | Incorpora qualquer dependência de terceiros | Gerado pelo `ui.frontend` módulo |

1. Observe que `clientlib-site` e `clientlib-dependencies` são ignorados do controle de origem. Isso ocorre por design, já que são gerados no momento da criação pelo `ui.frontend` módulo.

## Atualizar estilos base {#base-styles}

Em seguida, atualize os estilos básicos definidos no **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** módulo. Os arquivos no `ui.frontend` o módulo gera o `clientlib-site` e `clientlib-dependecies` Bibliotecas que contêm o tema do site e qualquer dependência de terceiros.

As bibliotecas do lado do cliente não são compatíveis com idiomas mais avançados, como [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Há várias ferramentas de código aberto, como [NPM](https://www.npmjs.com/) e [webpack](https://webpack.js.org/) que aceleram e otimizam o desenvolvimento de front-end. A meta do **ui.frontend** O módulo é capaz de usar essas ferramentas para gerenciar a maioria dos arquivos de origem front-end.

1. Abra o **ui.frontend** módulo e navegue até `src/main/webpack/site`.
1. Abra o arquivo `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` é o ponto de entrada para os arquivos Sass na `ui.frontend` módulo. Inclui a `_variables.scss` arquivo, que contém uma série de variáveis de marca a serem usadas em diferentes arquivos Sass no projeto. A variável `_base.scss` O arquivo também está incluído e define alguns estilos básicos para elementos HTML. Uma expressão regular inclui os estilos de componentes individuais em `src/main/webpack/components`. Outra expressão regular inclui os arquivos em `src/main/webpack/site/styles`.

1. Inspect o arquivo `main.ts`. Inclui `main.scss` e uma expressão regular para coletar qualquer `.js` ou `.ts` arquivos no projeto. Este ponto de entrada é usado pelo [arquivos de configuração do webpack](https://webpack.js.org/configuration/) como ponto de entrada para todo o `ui.frontend` módulo.

1. Inspect os arquivos abaixo de `src/main/webpack/site/styles`:

   ![Arquivos de estilo](assets/client-side-libraries/style-files.png)

   Esses arquivos definem estilos para elementos globais no modelo, como o Cabeçalho, Rodapé e o contêiner de conteúdo principal. As regras CSS nesses arquivos se destinam a diferentes elementos de HTML `header`, `main`, e  `footer`. Esses elementos de HTML foram definidos por políticas no capítulo anterior [Páginas e modelos](./pages-templates.md).

1. Expanda a `components` pasta em `src/main/webpack` e inspecione os arquivos.

   ![Arquivos Sass de componente](assets/client-side-libraries/component-sass-files.png)

   Cada arquivo mapeia para um Componente principal como o [Componente Acordeão](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). Cada Componente principal é criado com [Modificador do elemento do bloco](https://getbem.com/) ou notação BEM para facilitar o direcionamento a classes CSS específicas com regras de estilo. Os arquivos abaixo de `/components` foram compactados pelo Arquétipo de projeto AEM com as diferentes regras BEM para cada componente.

1. Baixar os estilos base WKND **[wknd-base-estilos-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** e **descompactar** o arquivo.

   ![Estilos de base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar o tutorial, vários arquivos Sass que implementam a marca WKND com base nos Componentes principais e na estrutura do Modelo de página do artigo são fornecidos.

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

## Integração do Inspect ao ui.frontend {#ui-frontend-integration}

Uma peça chave de integração incorporada no **ui.frontend** módulo, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) A pega os artefatos CSS e JS compilados de um projeto webpack/npm e os transforma em bibliotecas AEM do lado do cliente.

![Integração de arquitetura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

O Arquétipo de projeto AEM configura automaticamente essa integração. Em seguida, descubra como funciona.


1. Abra um terminal de linha de comando e instale o **ui.frontend** módulo usando o `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` a execução é necessária apenas uma vez, como após um novo clone ou geração do projeto.

1. Iniciar o servidor de desenvolvimento do webpack em **assistir** executando o seguinte comando:

   ```shell
   $ npm run watch
   ```

1. Isso compila os arquivos de origem a partir do `ui.frontend` e sincroniza as alterações com o AEM em [http://localhost:4502](http://localhost:4502)

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

1. O comando `npm run watch` preenche o **clientlib-site** e **clientlib-dependencies** no **ui.apps** que é sincronizado automaticamente com o AEM.

   >[!NOTE]
   >
   >Existe também uma `npm run prod` perfil que minifica o JS e o CSS. Essa é a compilação padrão sempre que a compilação do webpack é acionada por meio do Maven. Mais detalhes sobre o [O módulo ui.frontend pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect o arquivo `site.css` debaixo `ui.frontend/dist/clientlib-site/site.css`. Este é o CSS compilado com base nos arquivos de origem Sass.

   ![Css do site distribuído](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect o arquivo `ui.frontend/clientlib.config.js`. Este é o arquivo de configuração para um plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma o conteúdo de `/dist` em uma biblioteca do cliente e a move para a `ui.apps` módulo.

1. Inspect o arquivo `site.css` no **ui.apps** módulo em `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Esta deve ser uma cópia idêntica do `site.css` arquivo do **ui.frontend** módulo. Agora que está em **ui.apps** pode ser implantado no AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Desde **clientlib-site** é compilado durante o tempo de compilação, usando **npm** ou **maven**, ele pode ser ignorado com segurança do controle do código-fonte no **ui.apps** módulo. INSPECT o `.gitignore` arquivo abaixo **ui.apps**.

1. Abra o artigo LA Skatepark no AEM em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos base atualizados para o artigo](assets/client-side-libraries/updated-base-styles.png)

   Agora você deve ver os estilos atualizados do artigo. Talvez seja necessário fazer uma atualização permanente para limpar todos os arquivos CSS armazenados em cache pelo navegador.

   Está começando a parecer muito mais perto dos modelos!

   >[!NOTE]
   >
   > As etapas executadas acima para criar e implantar o código ui.frontend no AEM são executadas automaticamente quando uma criação Maven é acionada a partir da raiz do projeto `mvn clean install -PautoInstallSinglePackage`.

## Fazer uma alteração de estilo

Em seguida, faça uma pequena alteração no `ui.frontend` módulo para ver a `npm run watch` implante automaticamente os estilos na instância AEM local.

1. De, o `ui.frontend` módulo abra o arquivo: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Atualize o `$brand-primary` variável de cor:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Salve as alterações.

1. Retorne ao navegador e atualize a página AEM para ver as atualizações:

   ![Bibliotecas do cliente](assets/client-side-libraries/style-update-brand-primary.png)

1. Reverter a alteração para o `$brand-primary` colorir e parar a criação do webpack usando o comando `CTRL+C`.

>[!CAUTION]
>
> A utilização dos **ui.frontend** O módulo pode não ser necessário para todos os projetos. A variável **ui.frontend** O módulo adiciona mais complexidade e, se não houver necessidade/desejo de usar algumas dessas ferramentas avançadas de front-end (Sass, webpack, npm...), talvez ela não seja necessária.

## Inclusão de página e modelo {#page-inclusion}

Em seguida, vamos analisar como as clientlibs são referenciadas na página do AEM. Uma prática recomendada comum no desenvolvimento da Web é incluir o CSS no cabeçalho do HTML `<head>` e JavaScript logo antes de fechar `</body>` tag.

1. Navegue até o modelo Página de artigo em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Clique em **Informações da página** e, no menu, selecione **Política da página** para abrir o **Política da página** diálogo.

   ![Política da página de modelo de página de artigo](assets/client-side-libraries/template-page-policy.png)

   *Informações da página > Política da página*

1. Observe que as categorias para `wknd.dependencies` e `wknd.site` estão listados aqui. Por padrão, as clientlibs configuradas por meio da Política da página são divididas para incluir o CSS no cabeçalho da página e o JavaScript no final do corpo. Você pode listar explicitamente o JavaScript clientlib que deve ser carregado no cabeçalho da página. Este é o caso para `wknd.dependencies`.

   ![Política da página de modelo de página de artigo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > É igualmente possível fazer referência à `wknd.site` ou `wknd.dependencies` do componente de página diretamente, usando o `customheaderlibs.html` ou `customfooterlibs.html` script. Usar o modelo oferece flexibilidade para que você possa escolher quais clientlibs serão usadas por modelo. Por exemplo, se você tiver uma biblioteca JavaScript pesada que só será usada em um template selecionado.

1. Navegue até a **Los Angeles Skateparks** página criada usando o **Modelo da página de artigo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Clique em **Informações da página** e, no menu, selecione **Exibir como publicado** para abrir a página do artigo fora do Editor de AEM.

   ![Exibir como publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Visualizar a fonte da página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e você poderá ver as seguintes referências de clientlib no `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que clientlibs estão usando o proxy `/etc.clientlibs` terminal. Você também deve ver que a seguinte clientlib inclui na parte inferior da página:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Para o AEM 6.5/6.4, as bibliotecas do lado do cliente não são minificadas automaticamente. Consulte a documentação no [Gerenciador de biblioteca HTML para ativar minificação (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >No lado da publicação, é essencial que as bibliotecas do cliente sejam **não** servido por **/apps** pois esse caminho deve ser restrito por motivos de segurança usando o [Seção de filtro do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). A variável [propriedade allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) da biblioteca do cliente garante que o CSS e o JS sejam atendidos no **/etc.clientlibs**.

### Próximas etapas {#next-steps}

Saiba como implementar estilos individuais e reutilizar os Componentes principais usando o Sistema de estilos do Experience Manager. [Desenvolvimento com o sistema de estilo](style-system.md) A abrange o uso do Sistema de estilos para estender os Componentes principais com CSS específico da marca e configurações de política avançadas do Editor de modelos.

Exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/client-side-libraries-solution`.

1. Clonar o [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositório.
1. Confira o `tutorial/client-side-libraries-solution` filial.

## Ferramentas e recursos adicionais {#additional-resources}

### DevServer do Webpack - Marcação Estática {#webpack-dev-static}

Nos dois exercícios anteriores, vários arquivos Sass no **ui.frontend** Os módulos do foram atualizados e, por meio de um processo de criação, vemos que essas alterações foram refletidas no AEM. Em seguida, vamos analisar uma técnica que usa um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desenvolver rapidamente os estilos de front-end em relação **estático** HTML.

Essa técnica é útil se a maioria dos estilos e códigos de front-end for executada por um desenvolvedor de front-end dedicado que pode não ter acesso fácil a um ambiente AEM. Essa técnica também permite que o FED faça modificações diretamente no HTML, que podem então ser entregues a um desenvolvedor de AEM para serem implementados como componentes.

1. Copie a origem da página de artigo LA skatepark em [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Reabra o IDE. Cole a marcação copiada do AEM na `index.html` no **ui.frontend** módulo abaixo `src/main/webpack/static`.
1. Editar a marcação copiada e remover qualquer referência a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Remova essas referências porque o servidor de desenvolvimento do webpack gera esses artefatos automaticamente.

1. Inicie o servidor de desenvolvimento do webpack a partir de um novo terminal executando o seguinte comando no **ui.frontend** módulo:

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

1. Revise o `/aem-guides-wknd.ui.frontend/webpack.dev.js` arquivo. Contém a configuração de webpack usada para iniciar o webpack-dev-server. Ela faz proxy dos caminhos `/content` e `/etc.clientlibs` de uma instância de AEM em execução local. É assim que as imagens e outras bibliotecas de clientes (não gerenciadas pelo **ui.frontend** (código de segurança).

   >[!CAUTION]
   >
   > A origem de imagem da marcação estática aponta para um componente de imagem ativa em uma ocorrência de AEM local. As imagens aparecem quebradas se o caminho para a imagem mudar, se o AEM não for iniciado ou se o navegador não tiver se conectado à instância AEM local. Se estiver enviando para um recurso externo, também é possível substituir as imagens por referências estáticas.

1. Você pode **stop** o servidor do webpack na linha de comando digitando `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** O é uma ferramenta de linha de comando de código aberto que pode ser usada para acelerar o desenvolvimento de front-end. Ele é alimentado por [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/), e [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

A um nível elevado, a `aemfed`O foi projetado para acompanhar as alterações de arquivos na **ui.apps** e os sincroniza automaticamente diretamente com uma instância AEM em execução. Com base nas alterações, um navegador local é atualizado automaticamente, acelerando o desenvolvimento de front-end. Ele também foi criado para funcionar com o rastreador de log do Sling para exibir automaticamente todos os erros do lado do servidor diretamente no terminal.

Se você estiver trabalhando muito dentro do **ui.apps** , modificando scripts HTL e criando componentes personalizados, **aemfed** pode ser uma ferramenta poderosa para usar. [A documentação completa pode ser encontrada aqui](https://github.com/abmaonline/aemfed).

### Depuração de bibliotecas do lado do cliente {#debugging-clientlibs}

Utilização de diferentes métodos de **categorias** e **incorpora** para incluir várias bibliotecas de clientes, pode ser complicado solucionar problemas. O AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é **Reconstruir bibliotecas de clientes** que força o AEM a recompilar qualquer arquivo MENOS e gerar o CSS.

* [**Bibliotecas de despejo**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista as bibliotecas de clientes registradas na instância AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Testar saída**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permite que um usuário veja a saída de HTML esperada de clientlib includes com base na categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validação de dependências de bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - destaca todas as dependências ou categorias incorporadas que não podem ser encontradas. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruir bibliotecas de clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permite que um usuário force o AEM a reconstruir as bibliotecas de clientes ou invalidar o cache das bibliotecas de clientes. Essa ferramenta é eficaz ao desenvolver com MENOS, pois isso pode forçar o AEM a recompilar o CSS gerado. Em geral, é mais eficaz Invalidar caches e executar uma atualização de página do que reconstruir as bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruir biblioteca do cliente](assets/client-side-libraries/rebuild-clientlibs.png)
