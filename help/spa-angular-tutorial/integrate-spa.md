---
title: Integrar um SPA | Introdução ao Editor SPA AEM e Angular
description: Entenda como o código-fonte de um aplicativo de página única (SPA) gravado em Angular pode ser integrado a um projeto Adobe Experience Manager (AEM). Aprenda a usar ferramentas modernas de front-end, como a ferramenta CLI da Angular, para desenvolver rapidamente o SPA em relação à API modelo JSON AEM.
sub-product: sites
feature: SPA Editor
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
source-wordcount: '2202'
ht-degree: 0%

---


# Integrar um SPA {#integrate-spa}

Entenda como o código-fonte de um aplicativo de página única (SPA) gravado em Angular pode ser integrado a um projeto Adobe Experience Manager (AEM). Aprenda a usar ferramentas modernas de front-end, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente o SPA em relação à API modelo JSON AEM.

## Objetivo

1. Entenda como o projeto SPA é integrado ao AEM com bibliotecas do lado do cliente.
2. Saiba como usar um servidor de desenvolvimento local para desenvolvimento de front-end dedicado.
3. Explore o uso de um arquivo **proxy** e **mock** estático para desenvolvimento em relação à API modelo JSON AEM

## O que você vai criar

Este capítulo adicionará um componente simples `Header` ao SPA. No processo de criação deste componente estático, serão utilizadas várias abordagens AEM desenvolvimento de ZPE. `Header`

![Novo cabeçalho no AEM](./assets/integrate-spa/final-header-component.png)

*O SPA é estendido para adicionar um`Header`componente estático*

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um ambiente [de desenvolvimento](overview.md#local-dev-environment)local.

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

   Se estiver usando [AEM 6.x](overview.md#compatibility) , adicione o `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou fazer check-out do código localmente ao alternar para a ramificação `Angular/integrate-spa-solution`.

## Abordagem de integração {#integration-approach}

Dois módulos foram criados como parte do projeto AEM: `ui.apps` e `ui.frontend`.

O `ui.frontend` módulo é um projeto do [webpack](https://webpack.js.org/) que contém todo o código fonte SPA. A maioria do desenvolvimento e teste do SPA será feito no projeto do webpack. Quando uma compilação de produção é acionada, o SPA é criado e compilado usando o webpack. Os artefatos compilados (CSS e Javascript) são copiados no `ui.apps` módulo que é implantado no tempo de execução AEM.

![arquitetura de alto nível ui.front-end](assets/integrate-spa/ui-frontend-architecture.png)

*Uma descrição de alto nível da integração com o SPA.*

Informações adicionais sobre a compilação do Front-end podem ser [encontradas aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Integração do SPA com a Inspect {#inspect-spa-integration}

Em seguida, inspecione o `ui.frontend` módulo para entender o SPA que foi gerado automaticamente pelo tipo de arquivo [AEM Project](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. No IDE de sua escolha, abra o AEM Project para o WKND SPA. Este tutorial usará o código IDE [do](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)Visual Studio.

   ![VSCode - Projeto SPA AEM WKND](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspecione a `ui.frontend` pasta. Open the file `ui.frontend/package.json`

3. Em `dependencies` você deve ver vários relacionados a `@angular`:

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

   O `ui.frontend` módulo é um aplicativo [](https://angular.io) Angular gerado usando a ferramenta [CLI](https://angular.io/cli) Angular que inclui o roteamento.

4. Há também três dependências com prefixo `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Os módulos acima compõem o SDK [JS do Editor SPA](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) AEM e fornecem a funcionalidade para possibilitar o mapeamento de componentes SPA para AEM componentes.

5. No `package.json` arquivo, vários `scripts` estão definidos:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Esses scripts são baseados em comandos [](https://angular.io/cli/build) angulares CLI comuns, mas foram ligeiramente modificados para funcionar com o projeto AEM maior.

   `start` - executa o aplicativo Angular localmente usando um servidor da Web local. Foi atualizado para proxy do conteúdo da instância de AEM local.

   `build` - compila o aplicativo Angular para distribuição de produção. A adição do é responsável por copiar o SPA compilado no `&& clientlib` `ui.apps` módulo como uma biblioteca do lado do cliente durante uma compilação. O módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) é usado para facilitar isso.

   More details about the available scripts can be found [here](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect o arquivo `ui.frontend/clientlib.config.js`. Esse arquivo de configuração é usado pelo [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar como gerar a biblioteca do cliente.

7. Inspect o arquivo `ui.frontend/pom.xml`. Esse arquivo transforma a `ui.frontend` pasta em um módulo [](http://maven.apache.org/guides/mini/guide-multiple-modules.html)Maven. O `pom.xml` arquivo foi atualizado para usar o [plug-in](https://github.com/eirslett/frontend-maven-plugin) frontende-maven para **testar** e **criar** o SPA durante uma compilação Maven.

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

   `app.component.js` é o ponto de entrada da ZPE. `ModelManager` é fornecido pelo SDK JS do Editor SPA AEM. É responsável por chamar e injetar o `pageModel` (conteúdo JSON) no aplicativo.

## Adicionar um componente Cabeçalho {#header-component}

Em seguida, adicione um novo componente ao SPA e implante as alterações em uma instância AEM local para ver a integração.

1. Abra uma nova janela de terminal e navegue até a `ui.frontend` pasta:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Instalar [Angular CLI](https://angular.io/cli#installing-angular-cli) globalmenteEssa opção é usada para gerar componentes Angular, bem como para criar e servir o aplicativo Angular através do comando **ng** .

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > A versão de **@angular/cli** usada por este projeto é a **9.1.7**. Recomenda-se manter as versões Angular CLI sincronizadas.

3. Crie um novo `Header` componente executando o comando Angular CLI `ng generate component` de dentro da `ui.frontend` pasta.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Isso criará um esqueleto para o novo componente de Cabeçalho Angular em `ui.frontend/src/app/components/header`.

4. Abra o `aem-guides-wknd-spa` projeto no IDE de sua escolha. Navegue até a `ui.frontend/src/app/components/header` pasta.

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

   Observe que isso exibe o conteúdo estático, de modo que esse componente Angular não requer nenhum ajuste no padrão gerado `header.component.ts`.

6. Abra o arquivo **app.component.html** em `ui.frontend/src/app/app.component.html`. Adicione o `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Isso incluirá o `header` componente acima de todo o conteúdo da página.

7. Abra um novo terminal e navegue até a `ui.frontend` pasta e execute o `npm run build` comando:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navegue até a `ui.apps` pasta. Abaixo, `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` você deve ver os arquivos SPA compilados que foram copiados da`ui.frontend/build` pasta.

   ![Biblioteca do cliente gerada em ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Retorne ao terminal e navegue até a `ui.apps` pasta. Execute o seguinte comando Maven:

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

10. Abra uma guia do navegador e navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Agora você deve ver o conteúdo do `Header` componente sendo exibido no SPA.

   ![Implementação inicial do cabeçalho](assets/integrate-spa/initial-header-implementation.png)

   As etapas de **7 a 9** são executadas automaticamente ao disparar uma compilação Maven da raiz do projeto (ou seja, `mvn clean install -PautoInstallSinglePackage`). Agora você deve entender as noções básicas da integração entre o SPA e AEM bibliotecas do lado do cliente. Observe que você ainda pode editar e adicionar `Text` componentes no AEM, no entanto, o `Header` componente não é editável.

## Servidor de Desenvolvimento do Webpack - Proxy da API JSON {#proxy-json}

Como visto nos exercícios anteriores, a execução de uma criação e sincronização da biblioteca do cliente em uma instância local do AEM leva alguns minutos. Isso é aceitável para testes finais, mas não é ideal para a maioria do desenvolvimento da ZPE.

Um servidor [de desenvolvimento de](https://webpack.js.org/configuration/dev-server/) webpack pode ser usado para desenvolver rapidamente o SPA. O SPA é conduzido por um modelo JSON gerado pela AEM. Neste exercício, o conteúdo JSON de uma instância em execução do AEM será **enviado** por proxy para o servidor de desenvolvimento configurado pelo projeto [](https://angular.io/guide/build)Angular.

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

   O aplicativo [](https://angular.io/guide/build#proxying-to-a-backend-server) Angular fornece um mecanismo fácil para solicitações de API de proxy. Os padrões especificados em `context` são enviados por proxy `localhost:4502`, o AEM local de início rápido.

2. Abra o arquivo **index.html** em `ui.frontend/src/index.html`. Este é o arquivo HTML raiz usado pelo servidor dev.

   Observe que há uma entrada para `base href="/"`. A tag [](https://angular.io/guide/deployment#the-base-tag) base é essencial para que o aplicativo resolva URLs relativos.

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

   ![Servidor dev do Webpack - json proxy](assets/integrate-spa/webpack-dev-server-1.png)

   Você deve ver o mesmo conteúdo que em AEM, mas sem nenhum dos recursos de criação ativados.

5. Retorne ao IDE e crie uma nova pasta chamada `img` em `ui.frontend/src/assets`.
6. Baixe e adicione o seguinte logotipo WKND à `img` pasta:

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

   Save the changes to **header.component.html**.

8. Retorne ao navegador. Você deve ver imediatamente as alterações no aplicativo refletidas.

   ![Logotipo adicionado ao cabeçalho](assets/integrate-spa/added-logo-localhost.png)

   Você pode continuar fazendo atualizações de conteúdo em **AEM** e vê-las refletidas no servidor **de desenvolvimento de** webpack, já que estamos fazendo proxy do conteúdo. Observe que as alterações de conteúdo são visíveis somente no servidor **de desenvolvimento de** webpack.

9. Pare o servidor Web local com `ctrl+c` o terminal.

## Servidor de Desenvolvimento de Webpack - API JSON Mock {#mock-json}

Outra abordagem para o desenvolvimento rápido é usar um arquivo JSON estático para agir como o modelo JSON. Ao &quot;zombar&quot; do JSON, removemos a dependência de uma instância AEM local. Ele também permite que um desenvolvedor front-end atualize o modelo JSON para testar a funcionalidade e as alterações na API JSON que seriam implementadas posteriormente por um desenvolvedor back-end.

A configuração inicial do modelo JSON **requer uma instância** AEM local.

1. No navegador, navegue até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Este é o JSON exportado pelo AEM que está liderando o aplicativo. Copie a saída JSON.

2. Retorne ao IDE para navegar `ui.frontend/src` e adicione novas pastas denominadas **mocks** e **json** para corresponder à seguinte estrutura de pastas:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Create a new file named **en.model.json** beneath `ui.frontend/public/mocks/json`. Cole a saída JSON na **Etapa 1** aqui.

   ![Arquivo Json do Modelo Mock](assets/integrate-spa/mock-model-json-created.png)

4. Create a new file **proxy.mock.conf.json** beneath `ui.frontend`. Preencha o arquivo com o seguinte:

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

   Essa configuração de proxy regravará as solicitações que o start tiver `/content/wknd-spa-angular/us` com `/mocks/json` e servirá o arquivo JSON estático correspondente, por exemplo:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Abra o arquivo **angular.json**. Adicione uma nova configuração **dev** com uma matriz de **ativos** atualizada para fazer referência à pasta **mocks** criada.

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

   ![Pasta de atualização de ativos JSON Dev Angular](assets/integrate-spa/dev-assets-update-folder.png)

   A criação de uma configuração **dev** dedicada garante que a pasta **mocks** seja usada somente durante o desenvolvimento e nunca seja implantada em AEM em uma compilação de produção.

6. No arquivo **angular.json** , atualize a configuração **browserTarget** para usar a nova configuração **dev** :

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

   ![Atualização de desenvolvimento de compilação JSON angular](assets/integrate-spa/angular-json-build-dev-update.png)

7. Abra o arquivo `ui.frontend/package.json` e adicione um novo comando **start:mock** para fazer referência ao arquivo **proxy.mock.conf.json** .

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

8. Se estiver em execução, pare o servidor **dev do** webpack. Start o servidor **dev de** webpack usando o script **start:mock** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navegue até [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) e você deverá ver o mesmo SPA, mas o conteúdo agora está sendo extraído do arquivo **mock** JSON.

9. Faça uma pequena alteração no arquivo **en.model.json** criado anteriormente. O conteúdo atualizado deve ser refletido imediatamente no servidor **de desenvolvimento do** webpack.

   ![atualização json modelo de modelo](./assets/integrate-spa/webpack-mock-model.gif)

   Ser capaz de manipular o modelo JSON e ver os efeitos em um SPA ativo pode ajudar um desenvolvedor a entender a API do modelo JSON. Ela também permite que o desenvolvimento tanto de front-end quanto de back-end aconteça em paralelo.

## Adicionar estilos com Sass

Em seguida, algum estilo atualizado será adicionado ao projeto. Este projeto adicionará suporte ao [Sass](https://sass-lang.com/) para alguns recursos úteis, como variáveis.

1. Abra uma janela de terminal e interrompa o servidor **de desenvolvimento do** webpack se ele for iniciado. De dentro da `ui.frontend` pasta, digite o seguinte comando para atualizar o aplicativo Angular para processar arquivos **.scss** .

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

2. Instale `normalize-scss` para normalizar os estilos nos navegadores:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Retorne ao IDE e, abaixo, `ui.frontend/src` crie uma nova pasta chamada `styles`.
4. Crie um novo arquivo sob o `ui.frontend/src/styles` nome `_variables.scss` e preencha-o com as seguintes variáveis:

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

6. Atualize o arquivo **angular.json** e renomeie todas as referências ao **style.css** com **styles.scss**. Deve haver 3 referências.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Atualizar estilos de cabeçalho

Em seguida, adicione alguns estilos específicos da marca ao componente **Cabeçalho** usando Sass.

1. Start o servidor **dev de** webpack para ver os estilos sendo atualizados em tempo real:

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

3. Atualize **header.component.js** para referenciar **header.component.scss**:

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

4. Retorne ao navegador e ao servidor **dev do** webpack:

   ![Cabeçalho Estilo - servidor de desenvolvimento de webpack](assets/integrate-spa/styled-header.png)

   Agora você deve ver os estilos atualizados adicionados ao componente **Cabeçalho** .

## Implantar atualizações de SPA para AEM

As alterações feitas no **Cabeçalho** estão visíveis apenas no momento através do servidor **dev do** webpack. Implante o SPA atualizado para AEM para ver as alterações.

1. Pare o servidor **de desenvolvimento de** webpack.
2. Navegue até a raiz do projeto `/aem-guides-wknd-spa` e implante o projeto para AEM usando o Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Você deve ver o **cabeçalho** atualizado com o logotipo e os estilos aplicados:

   ![Cabeçalho atualizado em AEM](assets/integrate-spa/final-header-component.png)

   Agora que o SPA atualizado está em AEM, a criação pode continuar.

## Parabéns! {#congratulations}

Parabéns, você atualizou o SPA e explorou a integração com o AEM! Agora você conhece duas abordagens diferentes para desenvolver o SPA em relação à API do modelo JSON AEM usando um servidor **de desenvolvimento de** webpack.

Você sempre pode visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou fazer check-out do código localmente ao alternar para a ramificação `Angular/integrate-spa-solution`.

### Próximas etapas {#next-steps}

[Mapear componentes SPA para AEM componentes](map-components.md) - Saiba como mapear componentes angulares para componentes Adobe Experience Manager (AEM) com o SDK JS do editor SPA AEM. O mapeamento de componentes permite que os autores façam atualizações dinâmicas para componentes SPA no Editor SPA AEM, de modo semelhante à criação AEM tradicional.
