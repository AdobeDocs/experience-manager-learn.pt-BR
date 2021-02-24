---
title: Integrar um SPA | Introdução ao AEM SPA Editor e Angular
description: Entenda como o código-fonte de um aplicativo de página única (SPA) gravado no Angular pode ser integrado a um projeto Adobe Experience Manager (AEM). Aprenda a usar as modernas ferramentas de front-end, como a ferramenta CLI do Angular, para desenvolver rapidamente a SPA contra a API do modelo JSON AEM.
sub-product: sites
feature: Editor SPA
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2204'
ht-degree: 0%

---


# Integrar um SPA {#integrate-spa}

Entenda como o código-fonte de um aplicativo de página única (SPA) gravado no Angular pode ser integrado a um projeto Adobe Experience Manager (AEM). Aprenda a usar ferramentas modernas de front-end, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente a SPA contra a API modelo JSON AEM.

## Objetivo

1. Entenda como o projeto SPA é integrado ao AEM com bibliotecas do lado do cliente.
2. Saiba como usar um servidor de desenvolvimento local para desenvolvimento de front-end dedicado.
3. Explore o uso de um arquivo **proxy** e **mock** estático para desenvolvimento em relação à API de modelo JSON AEM

## O que você vai criar

Este capítulo adicionará um componente `Header` simples ao SPA. No processo de criação desse componente estático `Header`, várias abordagens para AEM desenvolvimento SPA serão usadas.

![Novo cabeçalho no AEM](./assets/integrate-spa/final-header-component.png)

*A SPA é estendida para adicionar um  `Header` componente estático*

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Implante a base de código para uma instância AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou fazer check-out do código localmente ao alternar para a ramificação `Angular/integrate-spa-solution`.

## Abordagem de integração {#integration-approach}

Dois módulos foram criados como parte do projeto AEM: `ui.apps` e `ui.frontend`.

O módulo `ui.frontend` é um projeto [webpack](https://webpack.js.org/) que contém todo o código fonte SPA. A maioria do desenvolvimento e teste SPA serão feitos no projeto do webpack. Quando uma compilação de produção é acionada, a SPA é criada e compilada usando o webpack. Os artefatos compilados (CSS e Javascript) são copiados no módulo `ui.apps` que é implantado no tempo de execução AEM.

![arquitetura de alto nível ui.front-end](assets/integrate-spa/ui-frontend-architecture.png)

*Uma descrição de alto nível da integração SPA.*

Informações adicionais sobre a compilação do Front-end podem ser [encontradas aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect a integração SPA {#inspect-spa-integration}

Em seguida, inspecione o módulo `ui.frontend` para entender a SPA que foi gerada automaticamente pelo [AEM Tipo de arquivo do Project](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. No IDE de sua escolha, abra o AEM Project para a SPA WKND. Este tutorial usará o código do Visual Studio [IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM Projeto SPA WKND](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspecione a pasta `ui.frontend`. Abra o arquivo `ui.frontend/package.json`

3. Em `dependencies`, você deve ver vários relacionados a `@angular`:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   O módulo `ui.frontend` é um [aplicativo de Angular](https://angular.io) gerado usando a [ferramenta CLI de Angular](https://angular.io/cli) que inclui o roteamento.

4. Há também três dependências com prefixo `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Os módulos acima compõem o [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) e fornecem a funcionalidade para tornar possível mapear SPA componentes para AEM componentes.

5. No arquivo `package.json`, vários `scripts` são definidos:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Esses scripts são baseados em comandos CLI comuns [do Angular](https://angular.io/cli/build), mas foram ligeiramente modificados para funcionar com o projeto AEM maior.

   `start` - executa o aplicativo Angular localmente usando um servidor da Web local. Foi atualizado para proxy do conteúdo da instância de AEM local.

   `build` - compila o aplicativo Angular para distribuição de produção. A adição de `&& clientlib` é responsável por copiar o SPA compilado no módulo `ui.apps` como uma biblioteca do lado do cliente durante uma compilação. O módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) é usado para facilitar isso.

   Mais detalhes sobre os scripts disponíveis podem ser encontrados [aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect o arquivo `ui.frontend/clientlib.config.js`. Esse arquivo de configuração é usado por [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar como gerar a biblioteca do cliente.

7. Inspect o arquivo `ui.frontend/pom.xml`. Esse arquivo transforma a pasta `ui.frontend` em um [módulo Maven](http://maven.apache.org/guides/mini/guide-multiple-modules.html). O arquivo `pom.xml` foi atualizado para usar os SPA [frontende-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **test** e **build** durante uma compilação Maven.

8. Inspect o arquivo `app.component.ts` em `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` é o ponto de entrada do SPA. `ModelManager` é fornecido pelo AEM SPA Editor JS SDK. É responsável por chamar e injetar o `pageModel` (o conteúdo JSON) no aplicativo.

## Adicionar um componente de cabeçalho {#header-component}

Em seguida, adicione um novo componente ao SPA e implante as alterações em uma instância AEM local para ver a integração.

1. Abra uma nova janela de terminal e navegue até a pasta `ui.frontend`:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Instale [CLI](https://angular.io/cli#installing-angular-cli) globalmente o Angular é usado para gerar componentes do Angular, bem como para criar e servir o aplicativo do Angular através do comando **ng**.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > A versão de **@angular/cli** usada por este projeto é **9.1.7**. É recomendável manter as versões da CLI do Angular sincronizadas.

3. Crie um novo componente `Header` executando o comando CLI `ng generate component` do Angular na pasta `ui.frontend`.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Isso criará um esqueleto para o novo componente Cabeçalho do Angular em `ui.frontend/src/app/components/header`.

4. Abra o projeto `aem-guides-wknd-spa` no IDE de sua escolha. Navegue até a pasta `ui.frontend/src/app/components/header`.

   ![Caminho do componente de cabeçalho no IDE](assets/integrate-spa/header-component-path.png)

5. Abra o arquivo `header.component.html` e substitua o conteúdo pelo seguinte:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Observe que isso exibe o conteúdo estático, de modo que esse componente de Angular não requer nenhum ajuste no `header.component.ts` padrão gerado.

6. Abra o arquivo **app.component.html** em `ui.frontend/src/app/app.component.html`. Adicione `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Isso incluirá o componente `header` acima de todo o conteúdo da página.

7. Abra um novo terminal e navegue até a pasta `ui.frontend` e execute o comando `npm run build`:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navegue até a pasta `ui.apps`. Abaixo de `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular`, você deve ver os arquivos de SPA compilados que foram copiados da pasta`ui.frontend/build`.

   ![Biblioteca do cliente gerada em ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Retorne ao terminal e navegue até a pasta `ui.apps`. Execute o seguinte comando Maven:

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Isso implantará o pacote `ui.apps` em uma instância de execução local do AEM.

10. Abra uma guia do navegador e navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Agora você deve ver o conteúdo do componente `Header` sendo exibido no SPA.

   ![Implementação inicial do cabeçalho](assets/integrate-spa/initial-header-implementation.png)

   As etapas **7-9** são executadas automaticamente ao disparar uma compilação Maven da raiz do projeto (ou seja, `mvn clean install -PautoInstallSinglePackage`). Agora você deve entender as noções básicas da integração entre as bibliotecas do SPA e AEM do cliente. Observe que você ainda pode editar e adicionar `Text` componentes no AEM, no entanto, o componente `Header` não é editável.

## Servidor de Desenvolvimento do Webpack - Proxy da API JSON {#proxy-json}

Como visto nos exercícios anteriores, a execução de uma criação e sincronização da biblioteca do cliente em uma instância local do AEM leva alguns minutos. Isso é aceitável para testes finais, mas não é ideal para a maioria do desenvolvimento SPA.

Um [servidor dev de webpack](https://webpack.js.org/configuration/dev-server/) pode ser usado para desenvolver rapidamente o SPA. O SPA é conduzido por um modelo JSON gerado pela AEM. Neste exercício, o conteúdo JSON de uma instância em execução do AEM será **proxy** no servidor de desenvolvimento configurado pelo [Angular project](https://angular.io/guide/build).

1. Retorne ao IDE e abra o arquivo **proxy.conf.json** em `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   O [aplicativo Angular](https://angular.io/guide/build#proxying-to-a-backend-server) fornece um mecanismo fácil para solicitações de API de proxy. Os padrões especificados em `context` são enviados por proxy por `localhost:4502`, o AEM local de início rápido.

2. Abra o arquivo **index.html** em `ui.frontend/src/index.html`. Este é o arquivo HTML raiz usado pelo servidor dev.

   Observe que há uma entrada para `base href="/"`. A [tag base](https://angular.io/guide/deployment#the-base-tag) é essencial para que o aplicativo resolva URLs relativos.

   ```html
   <base href="/">
   ```

3. Abra uma janela de terminal e navegue até a pasta `ui.frontend`. Execute o comando `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. Abra uma nova guia do navegador (se ainda não estiver aberta) e navegue até [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Servidor dev do Webpack - json proxy](assets/integrate-spa/webpack-dev-server-1.png)

   Você deve ver o mesmo conteúdo que em AEM, mas sem nenhum dos recursos de criação ativados.

5. Retorne ao IDE e crie uma nova pasta chamada `img` em `ui.frontend/src/assets`.
6. Baixe e adicione o seguinte logotipo WKND à pasta `img`:

   ![Logotipo WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Abra **header.component.html** em `ui.frontend/src/app/components/header/header.component.html` e inclua o logotipo:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Salve as alterações em **header.component.html**.

8. Retorne ao navegador. Você deve ver imediatamente as alterações no aplicativo refletidas.

   ![Logotipo adicionado ao cabeçalho](assets/integrate-spa/added-logo-localhost.png)

   Você pode continuar fazendo atualizações de conteúdo em **AEM** e vê-las refletidas em **servidor dev de webpack**, já que estamos fazendo proxy do conteúdo. Observe que as alterações de conteúdo são visíveis somente no **servidor dev de webpack**.

9. Pare o servidor Web local com `ctrl+c` no terminal.

## Servidor de Desenvolvimento do Webpack - API JSON Mock {#mock-json}

Outra abordagem para o desenvolvimento rápido é usar um arquivo JSON estático para agir como o modelo JSON. Ao &quot;zombar&quot; do JSON, removemos a dependência de uma instância AEM local. Ele também permite que um desenvolvedor front-end atualize o modelo JSON para testar a funcionalidade e as alterações na API JSON que seriam implementadas posteriormente por um desenvolvedor back-end.

A configuração inicial do modelo JSON **requer uma instância AEM local**.

1. No navegador, navegue até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Este é o JSON exportado pelo AEM que está liderando o aplicativo. Copie a saída JSON.

2. Retorne ao IDE para navegar até `ui.frontend/src` e adicione novas pastas chamadas **mocks** e **json** para corresponder à seguinte estrutura de pastas:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Crie um novo arquivo chamado **en.model.json** abaixo de `ui.frontend/public/mocks/json`. Cole a saída JSON de **Etapa 1** aqui.

   ![Arquivo Json do Modelo Mock](assets/integrate-spa/mock-model-json-created.png)

4. Crie um novo arquivo **proxy.mock.conf.json** abaixo de `ui.frontend`. Preencha o arquivo com o seguinte:

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   Essa configuração de proxy regravará solicitações que start com `/content/wknd-spa-angular/us` com `/mocks/json` e servirá o arquivo JSON estático correspondente, por exemplo:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Abra o arquivo **angular.json**. Adicione uma nova configuração **dev** com uma matriz **assets** atualizada para fazer referência à pasta **mocks** criada.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Pasta de atualização dos ativos de desenvolvimento JSON do angular](assets/integrate-spa/dev-assets-update-folder.png)

   A criação de uma configuração dedicada **dev** garante que a pasta **mocks** seja usada somente durante o desenvolvimento e nunca seja implantada em AEM em uma compilação de produção.

6. No arquivo **angular.json**, atualize a configuração **browserTarget** para usar a nova configuração **dev**:

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Atualização de desenvolvimento de compilação JSON de angular](assets/integrate-spa/angular-json-build-dev-update.png)

7. Abra o arquivo `ui.frontend/package.json` e adicione um novo comando **start:mock** para fazer referência ao arquivo **proxy.mock.conf.json**.

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   Adicionar um novo comando facilita alternar entre as configurações de proxy.

8. Se estiver em execução, pare o **servidor dev de webpack**. Start o **servidor dev de webpack** usando o script **start:mock**:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navegue até [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) e você deverá ver o mesmo SPA, mas o conteúdo agora está sendo extraído do arquivo JSON **mock**.

9. Faça uma pequena alteração no arquivo **en.model.json** criado anteriormente. O conteúdo atualizado deve ser refletido imediatamente no **servidor dev de webpack**.

   ![atualização json modelo de modelo](./assets/integrate-spa/webpack-mock-model.gif)

   Ser capaz de manipular o modelo JSON e ver os efeitos em um SPA ao vivo pode ajudar um desenvolvedor a entender a API do modelo JSON. Ela também permite que o desenvolvimento tanto de front-end quanto de back-end aconteça em paralelo.

## Adicionar estilos com Sass

Em seguida, algum estilo atualizado será adicionado ao projeto. Este projeto adicionará suporte [Sass](https://sass-lang.com/) para alguns recursos úteis, como variáveis.

1. Abra uma janela de terminal e pare o **servidor dev de webpack**, se iniciado. Na pasta `ui.frontend`, digite o seguinte comando para atualizar o aplicativo do Angular para processar os arquivos **.scss**.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Isso atualizará o arquivo `angular.json` com uma nova entrada na parte inferior do arquivo:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Instale `normalize-scss` para normalizar os estilos nos navegadores:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Retorne ao IDE e, abaixo de `ui.frontend/src` crie uma nova pasta chamada `styles`.
4. Crie um novo arquivo sob `ui.frontend/src/styles` chamado `_variables.scss` e preencha-o com as seguintes variáveis:

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. Nomeie novamente a extensão do arquivo **styles.css** em `ui.frontend/src/styles.css` para **styles.scss**. Substitua o conteúdo pelo seguinte:

   ```scss
   /* styles.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. Atualize **angular.json** e renomeie todas as referências a **style.css** com **styles.scss**. Deve haver 3 referências.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Atualizar estilos de cabeçalho

Em seguida, adicione alguns estilos específicos da marca ao componente **Header** usando Sass.

1. Start o **servidor dev de webpack** para ver os estilos a atualizar em tempo real:

   ```shell
   $ npm run start:mock
   ```

2. Em `ui.frontend/src/app/components/header` renomeie **header.component.css** para **header.component.scss**. Preencha o arquivo com o seguinte:

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. Atualize **header.component.js** para fazer referência a **header.component.scss**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. Retorne ao navegador e ao **servidor dev de webpack**:

   ![Cabeçalho Estilo - servidor de desenvolvimento de webpack](assets/integrate-spa/styled-header.png)

   Agora você deve ver os estilos atualizados adicionados ao componente **Header**.

## Implantar atualizações SPA para AEM

As alterações feitas no **Cabeçalho** estão visíveis apenas no momento pelo **servidor dev de webpack**. Implante o SPA atualizado para AEM para ver as alterações.

1. Pare o **servidor dev de webpack**.
2. Navegue até a raiz do projeto `/aem-guides-wknd-spa` e implante o projeto para AEM usando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Você deve ver o **Cabeçalho** atualizado com o logotipo e os estilos aplicados:

   ![Cabeçalho atualizado em AEM](assets/integrate-spa/final-header-component.png)

   Agora que a SPA atualizada está em AEM, a criação pode continuar.

## Parabéns! {#congratulations}

Parabéns, você atualizou o SPA e explorou a integração com o AEM! Agora você conhece duas abordagens diferentes para desenvolver o SPA em relação à API do modelo JSON AEM usando um **servidor dev de webpack**.

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou fazer check-out do código localmente ao alternar para a ramificação `Angular/integrate-spa-solution`.

### Próximas etapas {#next-steps}

[Mapear componentes SPA para AEM componentes](map-components.md)  - saiba como mapear componentes de Angular para componentes do Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os autores façam atualizações dinâmicas para SPA componentes dentro do AEM SPA Editor, de forma semelhante à criação tradicional AEM.
