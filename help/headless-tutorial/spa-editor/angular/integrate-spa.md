---
title: Integrar um SPA | Introdução ao AEM SPA Editor e Angular
description: Entenda como o código-fonte de um Aplicativo de página única (SPA) gravado no Angular pode ser integrado a um Projeto do Adobe Experience Manager (AEM). Aprenda a usar ferramentas de front-end modernas, como a ferramenta CLI do Angular, para desenvolver rapidamente a SPA em relação à API do modelo JSON AEM.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2191'
ht-degree: 0%

---

# Integrar um SPA {#integrate-spa}

Entenda como o código-fonte de um Aplicativo de página única (SPA) gravado no Angular pode ser integrado a um Projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas de front-end modernas, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente a SPA em relação à API de modelo JSON AEM.

## Objetivo

1. Entenda como o projeto do SPA é integrado ao AEM com bibliotecas do lado do cliente.
2. Saiba como usar um servidor de desenvolvimento local para desenvolvimento front-end dedicado.
3. Explore o uso de um **proxy** e estática **zombaria** arquivo para desenvolvimento em relação à API do modelo JSON do AEM

## O que você vai criar

Este capítulo adicionará uma `Header` para o SPA. No processo de criação dessa estática `Header` diversas abordagens AEM desenvolvimento SPA serão usadas.

![Novo Cabeçalho no AEM](./assets/integrate-spa/final-header-component.png)

*O SPA é estendido para adicionar um estático `Header` componente*

## Pré-requisitos

Revise as ferramentas necessárias e as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Implante a base de código em uma instância de AEM local usando o Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou verifique o código localmente, alternando para a ramificação `Angular/integrate-spa-solution`.

## Abordagem de integração {#integration-approach}

Dois módulos foram criados como parte do projeto AEM: `ui.apps` e `ui.frontend`.

O `ui.frontend` é um [webpack](https://webpack.js.org/) projeto que contém todo o código-fonte SPA. A maior parte do desenvolvimento e teste de SPA será feito no projeto do webpack. Quando uma build de produção é acionada, a SPA é criada e compilada usando o webpack. Os artefatos compilados (CSS e Javascript) são copiados para o `ui.apps` que é implantado no tempo de execução AEM.

![arquitetura de alto nível ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Uma descrição de alto nível da integração do SPA.*

Informações adicionais sobre a build de front-end podem ser [encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect a integração de SPA {#inspect-spa-integration}

Em seguida, inspecione o `ui.frontend` para entender o SPA gerado automaticamente pelo [Arquétipo de projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. No IDE de sua escolha, abra o AEM Project para a SPA WKND. Este tutorial usará o [Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM projeto SPA WKND](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspecione o `ui.frontend` pasta. Abra o arquivo `ui.frontend/package.json`

3. Em `dependencies` você deve ver vários relacionados ao `@angular`:

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

   O `ui.frontend` é um [aplicativo Angular](https://angular.io) gerado pelo uso da variável [Ferramenta CLI do Angular](https://angular.io/cli) que inclui roteamento.

4. Há também três dependências com o prefixo `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Os módulos acima compõem a variável [SDK JS do Editor SPA AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) e fornecer a funcionalidade para possibilitar o mapeamento SPA componentes para AEM componentes.

5. No `package.json` arquivo vários `scripts` são definidas:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Esses scripts são baseados em [Comandos CLI do Angular](https://angular.io/cli/build) mas foram ligeiramente modificadas para funcionar com o projeto AEM maior.

   `start` - executa o aplicativo Angular localmente usando um servidor da Web local. Ele foi atualizado para proxy no conteúdo da instância de AEM local.

   `build` - compila o aplicativo Angular para distribuição de produção. A adição de `&& clientlib` é responsável por copiar a SPA compilada no `ui.apps` como uma biblioteca do lado do cliente durante uma build. O módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) é usada para facilitar isso.

   Mais detalhes sobre os scripts disponíveis podem ser encontrados [here](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect o arquivo `ui.frontend/clientlib.config.js`. Esse arquivo de configuração é usado por [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar como gerar a biblioteca do cliente.

7. Inspect o arquivo `ui.frontend/pom.xml`. Esse arquivo transforma a variável `ui.frontend` em uma [Módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). O `pom.xml` O arquivo foi atualizado para usar a variável [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **teste** e **build** o SPA durante uma compilação Maven.

8. Inspect o arquivo `app.component.ts` at `ui.frontend/src/app/app.component.ts`:

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

   `app.component.js` é o ponto de entrada do SPA. `ModelManager` é fornecido pelo AEM Editor JS SDK. É responsável por chamar e injetar a vacina `pageModel` (o conteúdo JSON) no aplicativo.

## Adicionar um componente Cabeçalho {#header-component}

Em seguida, adicione um novo componente ao SPA e implante as alterações em uma instância de AEM local para ver a integração.

1. Abra uma nova janela de terminal e navegue até a `ui.frontend` pasta:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Instalar [CLI do Angular](https://angular.io/cli#installing-angular-cli) globalmente É usado para gerar componentes do Angular, bem como para criar e servir o aplicativo do Angular por meio da variável **ng** comando.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > A versão de **@angular/cli** usado por este projeto é **9.1.7**. É recomendável manter as versões da CLI do Angular sincronizadas.

3. Crie um novo `Header` executando a CLI do Angular `ng generate component` de dentro do `ui.frontend` pasta.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Isso criará um esqueleto para o novo componente Cabeçalho do Angular em `ui.frontend/src/app/components/header`.

4. Abra o `aem-guides-wknd-spa` projeto no IDE de sua escolha. Navegue até o `ui.frontend/src/app/components/header` pasta.

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

   Observe que isso exibe o conteúdo estático, de modo que esse componente de Angular não requer ajustes no padrão gerado `header.component.ts`.

6. Abra o arquivo **app.component.html** at  `ui.frontend/src/app/app.component.html`. Adicione o `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Isso incluirá a variável `header` componente acima de todo o conteúdo da página.

7. Abra um novo terminal e navegue até o `ui.frontend` e execute o `npm run build` comando:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navegue até o `ui.apps` pasta. Beneath `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` você deve ver que os arquivos de SPA compilados foram copiados do`ui.frontend/build` pasta.

   ![Biblioteca de clientes gerada em ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Retorne ao terminal e navegue até o `ui.apps` pasta. Execute o seguinte comando Maven:

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

   Isso implantará o `ui.apps` pacote em uma instância de execução local do AEM.

10. Abra uma guia do navegador e navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Agora você deve ver o conteúdo da variável `Header` componente sendo exibido no SPA.

   ![Implementação do cabeçalho inicial](assets/integrate-spa/initial-header-implementation.png)

   Etapas **7-9** são executadas automaticamente ao acionar uma build Maven da raiz do projeto (ou seja, `mvn clean install -PautoInstallSinglePackage`). Agora você deve entender as noções básicas da integração entre o SPA e AEM bibliotecas do lado do cliente. Observe que você ainda pode editar e adicionar `Text` componentes no AEM, no entanto, a variável `Header` não é editável.

## Servidor de desenvolvimento de Webpack - Proxy da API JSON {#proxy-json}

Como visto nos exercícios anteriores, a execução de uma build e sincronização da biblioteca do cliente com uma instância local de AEM demora alguns minutos. Isso é aceitável para testes finais, mas não é ideal para a maioria do desenvolvimento SPA.

A [servidor de desenvolvimento de webpack](https://webpack.js.org/configuration/dev-server/) pode ser utilizado para desenvolver rapidamente a SPA. O SPA é conduzido por um modelo JSON gerado pelo AEM. Neste exercício, o conteúdo JSON de uma instância em execução de AEM será **proxied** no servidor de desenvolvimento configurado pelo [Projeto Angular](https://angular.io/guide/build).

1. Retorne ao IDE e abra o arquivo **proxy.conf.json** at `ui.frontend/proxy.conf.json`.

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

   O [aplicativo Angular](https://angular.io/guide/build#proxying-to-a-backend-server) O fornece um mecanismo fácil para solicitações de API proxy. Os padrões especificados em `context` serão enviadas por proxy `localhost:4502`, o AEM local quickstart.

2. Abra o arquivo **index.html** at `ui.frontend/src/index.html`. Este é o arquivo HTML raiz usado pelo servidor dev.

   Observe que há uma entrada para `base href="/"`. O [base tag](https://angular.io/guide/deployment#the-base-tag) é essencial para que o aplicativo resolva URLs relativos.

   ```html
   <base href="/">
   ```

3. Abra uma janela de terminal e navegue até a `ui.frontend` pasta. Execute o comando `npm start`:

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

   ![Servidor de desenvolvimento do Webpack - json proxy](assets/integrate-spa/webpack-dev-server-1.png)

   Você deve ver o mesmo conteúdo da AEM, mas sem qualquer recurso de criação ativado.

5. Retorne ao IDE e crie uma nova pasta chamada `img` at `ui.frontend/src/assets`.
6. Baixe e adicione o seguinte logotipo WKND ao `img` pasta:

   ![Logotipo da WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Abrir **header.component.html** at `ui.frontend/src/app/components/header/header.component.html` e incluir o logotipo:

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

   Você pode continuar fazendo atualizações de conteúdo no **AEM** e vê-los refletidos em **servidor de desenvolvimento de webpack**, já que estamos enviando o conteúdo por proxy. Observe que as alterações de conteúdo são visíveis apenas na variável **servidor de desenvolvimento de webpack**.

9. Pare o servidor Web local com `ctrl+c` no terminal.

## Servidor de Desenvolvimento de Webpack - API JSON Mock {#mock-json}

Outra abordagem para o desenvolvimento rápido é usar um arquivo JSON estático para agir como o modelo JSON. Ao &quot;zombar&quot; o JSON, removemos a dependência de uma instância de AEM local. Ele também permite que um desenvolvedor de front-end atualize o modelo JSON para testar a funcionalidade e direcionar alterações para a API JSON que seriam implementadas posteriormente por um desenvolvedor de back-end.

A configuração inicial do mock JSON faz **requer uma instância de AEM local**.

1. No navegador, navegue até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Este é o JSON exportado pelo AEM que está liderando o aplicativo. Copie a saída JSON.

2. Retorne ao IDE e navegue até `ui.frontend/src` e adicionar novas pastas nomeadas **mocha** e **json** para corresponder à seguinte estrutura de pastas:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Crie um novo arquivo com o nome **en.model.json** debaixo `ui.frontend/public/mocks/json`. Cole a saída JSON de **Etapa 1** aqui.

   ![Arquivo Json do Modelo de Mock](assets/integrate-spa/mock-model-json-created.png)

4. Criar um novo arquivo **proxy.mock.conf.json** debaixo `ui.frontend`. Preencha o arquivo com o seguinte:

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

   Essa configuração de proxy reescreverá solicitações que começam com `/content/wknd-spa-angular/us` com `/mocks/json` e sirva o arquivo JSON estático correspondente, por exemplo:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Abra o arquivo **angular.json**. Adicione um novo **dev** com uma configuração atualizada **ativos** matriz para fazer referência ao **mocha** pasta criada.

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

   ![Pasta de atualização de ativos de desenvolvimento JSON do Angular](assets/integrate-spa/dev-assets-update-folder.png)

   Criação de um **dev** a configuração garante que a variável **mocha** é usada somente durante o desenvolvimento e nunca é implantada em AEM em uma build de produção.

6. No **angular.json** arquivo , próximo atualize o **browserTarget** configuração para usar o novo **dev** configuração:

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

   ![Atualização de desenvolvimento de build JSON do angular](assets/integrate-spa/angular-json-build-dev-update.png)

7. Abra o arquivo `ui.frontend/package.json` e adicionar um novo **start:mock** para fazer referência ao **proxy.mock.conf.json** arquivo.

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

8. Se estiver em execução no momento, pare o **servidor de desenvolvimento de webpack**. Inicie o **servidor de desenvolvimento de webpack** usando o **start:mock** script:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navegar para [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) e você deve ver o mesmo SPA, mas o conteúdo está sendo extraído do **zombaria** Arquivo JSON.

9. Faça uma pequena alteração no **en.model.json** arquivo criado anteriormente. O conteúdo atualizado deve ser refletido imediatamente na variável **servidor de desenvolvimento de webpack**.

   ![modelo de modelo json atualizado](./assets/integrate-spa/webpack-mock-model.gif)

   Ser capaz de manipular o modelo JSON e ver os efeitos em um SPA ao vivo pode ajudar um desenvolvedor a entender a API do modelo JSON. Também permite que tanto o desenvolvimento front-end quanto o back-end aconteçam em paralelo.

## Adicionar estilos com o Sass

Em seguida, algum estilo atualizado será adicionado ao projeto. Este projeto adicionará [Sass](https://sass-lang.com/) suporte para alguns recursos úteis como variáveis.

1. Abra uma janela de terminal e pare o **servidor de desenvolvimento de webpack** se iniciado. De dentro da `ui.frontend` digite o seguinte comando para atualizar o aplicativo Angular para processar **.scss** arquivos.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Isso atualizará o `angular.json` arquivo com uma nova entrada na parte inferior do arquivo:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Instalar `normalize-scss` para normalizar os estilos nos navegadores:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Retorne ao IDE e abaixo `ui.frontend/src` crie uma nova pasta chamada `styles`.
4. Crie um novo arquivo abaixo de `ui.frontend/src/styles` nomeado `_variables.scss` e preencha com as seguintes variáveis:

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

5. Renomeie a extensão do arquivo **styles.css** at `ui.frontend/src/styles.css` para **styles.scss**. Substitua o conteúdo pelo seguinte:

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

6. Atualizar **angular.json** e renomeie todas as referências para **style.css** com **styles.scss**. Deve haver 3 referências.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Atualizar estilos de cabeçalho

Em seguida, adicione alguns estilos específicos de marca ao **Cabeçalho** componente usando o Sass.

1. Inicie o **servidor de desenvolvimento de webpack** para ver os estilos sendo atualizados em tempo real:

   ```shell
   $ npm run start:mock
   ```

2. Em `ui.frontend/src/app/components/header` renomear **header.component.css** para **header.component.scss**. Preencha o arquivo com o seguinte:

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

3. Atualizar **header.component.ts** referência **header.component.scss**:

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

4. Retorne ao navegador e a variável **servidor de desenvolvimento de webpack**:

   ![Cabeçalho estilizado - servidor de desenvolvimento de webpack](assets/integrate-spa/styled-header.png)

   Agora, você deve ver os estilos atualizados adicionados à variável **Cabeçalho** componente.

## Implantar SPA atualizações no AEM

As mudanças feitas no **Cabeçalho** atualmente, só são visíveis por meio da variável **servidor de desenvolvimento de webpack**. Implante o SPA atualizado para AEM para visualizar as alterações.

1. Pare o **servidor de desenvolvimento de webpack**.
2. Navegue até a raiz do projeto `/aem-guides-wknd-spa` e implantar o projeto no AEM usando o Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navegar para [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Você deve ver o **Cabeçalho** com logotipo e estilos aplicados:

   ![Cabeçalho atualizado em AEM](assets/integrate-spa/final-header-component.png)

   Agora que o SPA atualizado está em AEM, a criação pode continuar.

## Parabéns! {#congratulations}

Parabéns, você atualizou o SPA e explorou a integração com o AEM! Agora você sabe de duas abordagens diferentes para desenvolver o SPA em relação à API do modelo JSON AEM usando um **servidor de desenvolvimento de webpack**.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou verifique o código localmente, alternando para a ramificação `Angular/integrate-spa-solution`.

### Próximas etapas {#next-steps}

[Mapear componentes SPA para AEM componentes](map-components.md) - Saiba como mapear componentes do Angular para componentes do Adobe Experience Manager (AEM) com o SDK JS do AEM SPA Editor. O mapeamento de componentes permite que os autores façam atualizações dinâmicas em componentes SPA no Editor de SPA de AEM, de forma semelhante à criação de AEM tradicional.
