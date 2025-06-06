---
title: Bibliotecas de clientes e fluxo de trabalho de front-end
description: Saiba como usar bibliotecas de clientes para implantar e gerenciar CSS e JavaScript para uma implementação do Adobe Experience Manager (AEM) Sites. Saiba como o módulo ui.frontend, um projeto de webpack, pode ser integrado ao processo de build completo.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 1%

---

# Bibliotecas de clientes e fluxo de trabalho de front-end {#client-side-libraries}

Saiba como as bibliotecas do lado do cliente ou clientlibs são usadas para implantar e gerenciar o CSS e o JavaScript para uma implementação do Adobe Experience Manager (AEM) Sites. Este tutorial também aborda como o módulo [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=pt-BR), um projeto [webpack](https://webpack.js.org/) dissociado, pode ser integrado ao processo de compilação completo.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

Também é recomendável revisar o tutorial [Noções básicas sobre componentes](component-basics.md#client-side-libraries) para entender os fundamentos das bibliotecas do lado do cliente e do AEM.

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/client-side-libraries-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implante a base de código em uma instância do AEM local usando suas habilidades do Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) ou conferir o código localmente alternando para a ramificação `tutorial/client-side-libraries-solution`.

## Objetivos

1. Entenda como as bibliotecas do lado do cliente são incluídas em uma página por meio de um modelo editável.
1. Saiba como usar o módulo `ui.frontend` e um servidor de desenvolvimento de webpack para desenvolvimento front-end dedicado.
1. Entenda o fluxo de trabalho completo de entrega de CSS e JavaScript compilados para uma implementação do Sites.

## O que você vai criar {#what-build}

Neste capítulo, você adiciona alguns estilos de linha de base para o site WKND e o Modelo de Página de Artigo para aproximar a implementação dos [modelos de design de interface](assets/pages-templates/wknd-article-design.xd). Você usa um fluxo de trabalho avançado de front-end para integrar um projeto de webpack em uma biblioteca de cliente AEM.

![Estilos Concluídos](assets/client-side-libraries/finished-styles.png)

*Página de artigo com estilos de linha de base aplicados*

## Fundo {#background}

As bibliotecas do lado do cliente fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As metas básicas para bibliotecas do lado do cliente ou clientlibs são:

1. Armazene CSS/JS em arquivos pequenos separados para facilitar o desenvolvimento e a manutenção
1. Gerenciar dependências em estruturas de terceiros de maneira organizada
1. Minimize o número de solicitações do lado do cliente concatenando CSS/JS em uma ou duas solicitações.

Mais informações sobre o uso de [Bibliotecas do Lado do Cliente podem ser encontradas aqui.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR)

As bibliotecas do lado do cliente têm algumas limitações. Mais notavelmente, é um suporte limitado para linguagens de front-end populares como Sass, LESS e TypeScript. No tutorial, vamos ver como o módulo **ui.frontend** pode ajudar a resolver isso.

Implante a base de código inicial em uma instância do AEM local e navegue até [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página não tem estilo. Vamos implementar bibliotecas do lado do cliente para a marca WKND para adicionar CSS e JavaScript à página.

## Organização de bibliotecas do lado do cliente {#organization}

A seguir, vamos explorar a organização das clientlibs geradas pelo [Arquétipo de Projetos AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR).

![Organização de alto nível da biblioteca do cliente](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Organização da biblioteca do lado do cliente de diagrama de alto nível e inclusão da página*

>[!NOTE]
>
> A seguinte organização de biblioteca do lado do cliente é gerada pelo Arquétipo de projeto do AEM, mas representa apenas um ponto de partida. A forma como um projeto gerencia e fornece CSS e JavaScript para uma implementação do Sites pode variar bastante com base em recursos, conjuntos de habilidades e requisitos.

1. Usando o VSCode ou outro IDE, abra o módulo **ui.apps**.
1. Expanda o caminho `/apps/wknd/clientlibs` para exibir as clientlibs geradas pelo arquétipo.

   ![Clientlibs em ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Na seção abaixo, essas clientlibs são analisadas com mais detalhes.

1. A tabela a seguir resume as bibliotecas de clientes. Mais detalhes sobre [incluindo as Bibliotecas de Clientes podem ser encontrados aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=pt-BR#developing).

   | Nome | Descrição | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nível básico de CSS e JavaScript necessário para o funcionamento do Site WKND | incorpora bibliotecas de cliente dos Componentes principais |
   | `clientlib-grid` | Gera o CSS necessário para o [Modo de Layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=pt-BR) funcionar. | Os pontos de interrupção de dispositivo móvel/tablet podem ser configurados aqui |
   | `clientlib-site` | Contém um tema específico do site para o site WKND | Gerado pelo módulo `ui.frontend` |
   | `clientlib-dependencies` | Incorpora qualquer dependência de terceiros | Gerado pelo módulo `ui.frontend` |

1. Observe que `clientlib-site` e `clientlib-dependencies` são ignorados do controle do código-fonte. Isso ocorre por design, já que são gerados no momento da compilação pelo módulo `ui.frontend`.

## Atualizar estilos base {#base-styles}

Em seguida, atualize os estilos base definidos no módulo **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=pt-BR)**. Os arquivos no módulo `ui.frontend` geram as bibliotecas `clientlib-site` e `clientlib-dependecies` que contêm o tema do Site e quaisquer dependências de terceiros.

Bibliotecas do lado do cliente não dão suporte a linguagens mais avançadas como [Sass](https://sass-lang.com/) ou [TypeScript](https://www.typescriptlang.org/). Há várias ferramentas de código aberto, como o [NPM](https://www.npmjs.com/) e o [webpack](https://webpack.js.org/), que aceleram e otimizam o desenvolvimento de front-end. O objetivo do módulo **ui.frontend** é poder usar essas ferramentas para gerenciar a maioria dos arquivos de origem front-end.

1. Abra o módulo **ui.frontend** e navegue até `src/main/webpack/site`.
1. Abrir o arquivo `main.scss`

   ![main.scss - ponto de entrada](assets/client-side-libraries/main-scss.png)

   `main.scss` é o ponto de entrada para os arquivos Sass no módulo `ui.frontend`. Ele inclui o arquivo `_variables.scss`, que contém uma série de variáveis de marca a serem usadas em diferentes arquivos Sass no projeto. O arquivo `_base.scss` também está incluído e define alguns estilos básicos para elementos HTML. Uma expressão regular inclui os estilos de componentes individuais em `src/main/webpack/components`. Outra expressão regular inclui os arquivos em `src/main/webpack/site/styles`.

1. Inspecione o arquivo `main.ts`. Ele inclui `main.scss` e uma expressão regular para coletar quaisquer arquivos `.js` ou `.ts` no projeto. Este ponto de entrada é usado pelos [arquivos de configuração do webpack](https://webpack.js.org/configuration/) como ponto de entrada para todo o módulo `ui.frontend`.

1. Inspecionar os arquivos abaixo de `src/main/webpack/site/styles`:

   ![Arquivos de Estilo](assets/client-side-libraries/style-files.png)

   Esses arquivos definem estilos para elementos globais no modelo, como o Cabeçalho, Rodapé e o contêiner de conteúdo principal. As regras CSS nesses arquivos se destinam a diferentes elementos HTML `header`, `main` e `footer`. Estes elementos HTML foram definidos por políticas no capítulo anterior [Páginas e Modelos](./pages-templates.md).

1. Expanda a pasta `components` em `src/main/webpack` e inspecione os arquivos.

   ![Arquivos Sass de componente](assets/client-side-libraries/component-sass-files.png)

   Cada arquivo mapeia para um Componente Principal como o [Componente Acordeão](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=pt-BR). Cada Componente principal é criado com o [Modificador de elemento de bloco](https://getbem.com/) ou a notação BEM para facilitar o direcionamento a classes CSS específicas com regras de estilo. Os arquivos abaixo de `/components` foram compactados pelo Arquétipo de Projetos AEM com as diferentes regras BEM para cada componente.

1. Baixe o arquivo de Estilos Base WKND **[wknd-base-style-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** e **unzip**.

   ![Estilos de Base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar o tutorial, vários arquivos Sass que implementam a marca WKND com base nos Componentes principais e na estrutura do Modelo de página do artigo são fornecidos.

1. Substituir o conteúdo de `ui.frontend/src` por arquivos da etapa anterior. O conteúdo do zip deve substituir as seguintes pastas:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Arquivos alterados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspecione os arquivos alterados para ver detalhes da implementação do estilo WKND.

## Inspecionar a integração ui.frontend {#ui-frontend-integration}

Uma peça-chave de integração incorporada ao módulo **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) pega os artefatos CSS e JS compilados de um projeto webpack/npm e os transforma em bibliotecas AEM do lado do cliente.

![Integração de arquitetura ui.front-end](assets/client-side-libraries/ui-frontend-architecture.png)

O Arquétipo de projeto do AEM configura automaticamente essa integração. Em seguida, descubra como funciona.


1. Abra um terminal de linha de comando e instale o módulo **ui.frontend** usando o comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >A execução de `npm install` é necessária apenas uma vez, como depois de um novo clone ou geração do projeto.

1. Abra `ui.frontend/package.json` e no comando **scripts** **start** adicione `--env writeToDisk=true`.

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. Inicie o servidor de desenvolvimento do webpack no modo **watch** executando o seguinte comando:

   ```shell
   $ npm run watch
   ```

1. Isso compila os arquivos de origem do módulo `ui.frontend` e sincroniza as alterações com o AEM em [http://localhost:4502](http://localhost:4502)

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

1. O comando `npm run watch` preenche o **clientlib-site** e **clientlib-dependencies** no módulo **ui.apps** que é sincronizado automaticamente com o AEM.

   >[!NOTE]
   >
   >Também há um perfil `npm run prod` que minifica o JS e o CSS. Essa é a compilação padrão sempre que a compilação do webpack é acionada por meio do Maven. Mais detalhes sobre o módulo [ui.frontend podem ser encontrados aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=pt-BR).

1. Inspecione o arquivo `site.css` abaixo de `ui.frontend/dist/clientlib-site/site.css`. Este é o CSS compilado com base nos arquivos de origem Sass.

   ![Site Distribuído css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspecione o arquivo `ui.frontend/clientlib.config.js`. Este é o arquivo de configuração de um plug-in npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma o conteúdo de `/dist` em uma biblioteca do cliente e a move para o módulo `ui.apps`.

1. Inspecione o arquivo `site.css` no módulo **ui.apps** em `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Esta deve ser uma cópia idêntica do arquivo `site.css` do módulo **ui.frontend**. Agora que está no módulo **ui.apps**, ele pode ser implantado no AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Como o **clientlib-site** é compilado durante o tempo de compilação, usando **npm** ou **maven**, ele pode ser ignorado com segurança do controle do código-fonte no módulo **ui.apps**. Inspecione o arquivo `.gitignore` abaixo de **ui.apps**.

1. Abra o artigo LA Skatepark no AEM em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos Base Atualizados para o Artigo](assets/client-side-libraries/updated-base-styles.png)

   Agora você deve ver os estilos atualizados do artigo. Talvez seja necessário fazer uma atualização permanente para limpar todos os arquivos CSS armazenados em cache pelo navegador.

   Está começando a parecer muito mais perto dos modelos!

   >[!NOTE]
   >
   > As etapas executadas acima para criar e implantar o código ui.frontend no AEM são executadas automaticamente quando uma compilação Maven é acionada da raiz do projeto `mvn clean install -PautoInstallSinglePackage`.

## Fazer uma alteração de estilo

Em seguida, faça uma pequena alteração no módulo `ui.frontend` para ver o `npm run watch` implantar automaticamente os estilos na instância do AEM local.

1. No, o módulo `ui.frontend` abre o arquivo: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Atualize a variável de cor `$brand-primary`:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Salve as alterações.

1. Retorne ao navegador e atualize a página AEM para ver as atualizações:

   ![Bibliotecas do lado do cliente](assets/client-side-libraries/style-update-brand-primary.png)

1. Reverta a alteração para a cor `$brand-primary` e pare a compilação do webpack usando o comando `CTRL+C`.

>[!CAUTION]
>
> O uso do módulo **ui.frontend** pode não ser necessário para todos os projetos. O módulo **ui.frontend** adiciona mais complexidade e, se não houver necessidade/desejo de usar algumas dessas ferramentas avançadas de front-end (Sass, webpack, npm...), talvez ele não seja necessário.

## Inclusão de página e modelo {#page-inclusion}

Em seguida, vamos analisar como as clientlibs são referenciadas na página do AEM. Uma prática recomendada comum no desenvolvimento da Web é incluir CSS no Cabeçalho do HTML `<head>` e no JavaScript logo antes de fechar a tag `</body>`.

1. Navegue até o modelo de Página de Artigo em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Clique no ícone **Informações da página** e, no menu, selecione **Política da página** para abrir a caixa de diálogo **Política da página**.

   ![Política de Página de Modelo de Página de Artigo](assets/client-side-libraries/template-page-policy.png)

   *Informações da página > Política da página*

1. Observe que as categorias para `wknd.dependencies` e `wknd.site` estão listadas aqui. Por padrão, as clientlibs configuradas por meio da Política da página são divididas para incluir o CSS no cabeçalho da página e o JavaScript no final do corpo. Você pode listar explicitamente o JavaScript clientlib a ser carregado no Cabeçalho da página. Este é o caso para `wknd.dependencies`.

   ![Política de Página de Modelo de Página de Artigo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Também é possível referenciar `wknd.site` ou `wknd.dependencies` diretamente do componente de página, usando o script `customheaderlibs.html` ou `customfooterlibs.html`. Usar o modelo oferece flexibilidade para que você possa escolher quais clientlibs serão usadas por modelo. Por exemplo, se você tiver uma biblioteca JavaScript pesada que só será usada em um modelo selecionado.

1. Navegue até a página **LA Skateparks** criada usando o **Modelo de Página de Artigo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Clique no ícone **Informações da página** e, no menu, selecione **Exibir como publicada** para abrir a página do artigo fora do Editor do AEM.

   ![Exibir como publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Exiba a Origem da página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) e você poderá ver as seguintes referências de clientlib no `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que clientlibs estão usando o ponto de extremidade proxy `/etc.clientlibs`. Você também deve ver que a seguinte clientlib inclui na parte inferior da página:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Para o AEM 6.5/6.4, as bibliotecas do lado do cliente não são minificadas automaticamente. Consulte a documentação no [Gerenciador de bibliotecas da HTML para habilitar a minificação (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR#using-preprocessors).

   >[!WARNING]
   >
   >É crítico, no lado da publicação, que as bibliotecas de clientes sejam **não** atendidas de **/aplicativos**, pois esse caminho deve ser restrito por motivos de segurança usando a [seção de filtro do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=pt-BR#example-filter-section). A propriedade [allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) da biblioteca do cliente garante que o CSS e o JS sejam atendidos a partir de **/etc.clientlibs**.

### Próximas etapas {#next-steps}

Saiba como implementar estilos individuais e reutilizar os Componentes principais usando o Sistema de estilos do Experience Manager. [O Desenvolvimento com o Sistema de Estilos](style-system.md) abrange o uso do Sistema de Estilos para estender os Componentes Principais com CSS específico da marca e configurações de política avançadas do Editor de Modelos.

Visualize o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/client-side-libraries-solution`.

1. Clonar o repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/client-side-libraries-solution`.

## Ferramentas e recursos adicionais {#additional-resources}

### DevServer do Webpack - Marcação Estática {#webpack-dev-static}

No par de exercícios anteriores, vários arquivos Sass no módulo **ui.frontend** foram atualizados e, por meio de um processo de compilação, você pode ver que essas alterações foram refletidas no AEM. A seguir, vejamos uma técnica que usa um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desenvolver rapidamente os estilos de front-end em relação ao HTML **static**.

Essa técnica é útil se a maioria dos estilos e códigos de front-end for executada por um desenvolvedor de front-end dedicado que pode não ter acesso fácil a um ambiente do AEM. Essa técnica também permite que o FED faça modificações diretamente no HTML, que podem ser entregues a um desenvolvedor do AEM para implementação como componentes.

1. Copie a origem da página de artigo LA skatepark em [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Reabra o IDE. Cole a marcação copiada do AEM no `index.html` no módulo **ui.frontend** abaixo de `src/main/webpack/static`.
1. Edite a marcação copiada e remova todas as referências a **clientlib-site** e **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Remova essas referências porque o servidor de desenvolvimento do webpack gera esses artefatos automaticamente.

1. Inicie o servidor de desenvolvimento do webpack a partir de um novo terminal executando o seguinte comando no módulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Isso deve abrir uma nova janela do navegador em [http://localhost:8080/](http://localhost:8080/) com marcação estática.

1. Edite o arquivo `src/main/webpack/site/_variables.scss`. Substitua a regra `$text-color` pela seguinte:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Salve as alterações.

1. Você deve ver automaticamente as alterações refletidas automaticamente no navegador em [http://localhost:8080](http://localhost:8080).

   ![Alterações no servidor de desenvolvimento do webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise o arquivo `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contém a configuração de webpack usada para iniciar o webpack-dev-server. Ela faz proxy dos caminhos `/content` e `/etc.clientlibs` de uma instância do AEM em execução localmente. É assim que as imagens e outras clientlibs (não gerenciadas pelo código **ui.frontend**) são disponibilizadas.

   >[!CAUTION]
   >
   > A origem de imagem da marcação estática aponta para um componente de imagem em tempo real em uma instância do AEM local. As imagens aparecem quebradas se o caminho para a imagem mudar, se o AEM não for iniciado ou se o navegador não tiver se conectado à instância local do AEM. Se estiver enviando para um recurso externo, também é possível substituir as imagens por referências estáticas.

1. Você pode **parar** o servidor de webpack a partir da linha de comando digitando `CTRL+C`.

### Depuração de bibliotecas do lado do cliente {#debugging-clientlibs}

Usar diferentes métodos de **categorias** e **incorpora** para incluir várias bibliotecas de clientes, pode ser complicado solucionar problemas. O AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é **Recompilar Bibliotecas de Clientes**, que força o AEM a recompilar qualquer arquivo MENOS e gerar o CSS.

* [**Bibliotecas de Despejo**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista as bibliotecas de clientes registradas na instância do AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Saída de Teste**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permite que um usuário veja a saída de HTML esperada de clientlib includes com base na categoria. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validação de dependências de bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - destaca todas as dependências ou categorias inseridas que não podem ser encontradas. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Recompilar Bibliotecas de Clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permite que um usuário force a AEM a recompilar as bibliotecas de clientes ou invalidar o cache de bibliotecas de clientes. Essa ferramenta é eficaz ao desenvolver com MENOS, pois isso pode forçar o AEM a recompilar o CSS gerado. Em geral, é mais eficaz Invalidar caches e executar uma atualização de página do que reconstruir as bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![recompilar biblioteca do cliente](assets/client-side-libraries/rebuild-clientlibs.png)
