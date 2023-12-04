---
title: Integrar um SPA | Introdução ao SPA Editor e Angular do AEM
description: Entenda como o código-fonte de um aplicativo de página única (SPA) escrito no Angular pode ser integrado a um projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas modernas de front-end, como a ferramenta CLI do Angular, para desenvolver rapidamente o SPA contra a API do modelo JSON do AEM.
feature: SPA Editor
version: Cloud Service
jira: KT-5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
duration: 760
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2045'
ht-degree: 0%

---

# Integrar um SPA {#integrate-spa}

Entenda como o código-fonte de um aplicativo de página única (SPA) escrito no Angular pode ser integrado a um projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas de front-end modernas, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente o SPA contra a API do modelo AEM JSON.

## Objetivo

1. Entenda como o projeto SPA é integrado ao AEM com bibliotecas do lado do cliente.
2. Saiba como usar um servidor de desenvolvimento local para desenvolvimento front-end dedicado.
3. Explore o uso de um **proxy** e estática **modelo** arquivo para desenvolvimento em relação à API do modelo JSON AEM

## O que você vai criar

Este capítulo adicionará uma `Header` componente ao SPA. No processo de criação dessa estática `Header` AEM componente são utilizadas várias abordagens para o desenvolvimento do SPA.

![Novo cabeçalho no AEM](./assets/integrate-spa/final-header-component.png)

*O SPA é estendido para adicionar um estático `Header` componente*

## Pré-requisitos

Analisar as ferramentas e instruções necessárias para a configuração de um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial pelo Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Implante a base de código em uma instância de AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou verifique o código localmente alternando para a ramificação `Angular/integrate-spa-solution`.

## Abordagem de integração {#integration-approach}

Dois módulos foram criados como parte do projeto AEM: `ui.apps` e `ui.frontend`.

A variável `ui.frontend` o módulo é um [webpack](https://webpack.js.org/) projeto que contém todo o código-fonte SPA. A maioria do desenvolvimento e teste do SPA é feita no projeto webpack. Quando um build de produção é acionado, o SPA é criado e compilado usando o webpack. Os artefatos compilados (CSS e Javascript) são copiados na variável `ui.apps` que é então implantado no tempo de execução do AEM.

![Arquitetura de alto nível ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Uma descrição de alto nível da integração do SPA.*

Informações adicionais sobre a build de front-end podem ser [encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Integração do Inspect ao SPA {#inspect-spa-integration}

Em seguida, inspecione o `ui.frontend` módulo para entender o SPA que foi gerado automaticamente pelo [Arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. No IDE de sua escolha, abra o projeto AEM para o SPA WKND. Este tutorial usará o [IDE do Visual Studio Code](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - Projeto AEM WKND SPA](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspecione o `ui.frontend` pasta. Abra o arquivo `ui.frontend/package.json`

3. No `dependencies` você deve ver vários relacionados a `@angular`:

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

   A variável `ui.frontend` o módulo é um [aplicativo Angular](https://angular.io) gerado usando o [Ferramenta CLI do Angular](https://angular.io/cli) que inclui roteamento.

4. Há também três dependências com o prefixo `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   Os módulos acima compõem o [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) e fornecer a funcionalidade para possibilitar o mapeamento de componentes do SPA para componentes do AEM.

5. No `package.json` arquivo vários `scripts` são definidos:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Esses scripts são baseados em [Comandos da CLI do Angular](https://angular.io/cli/build) mas foram ligeiramente modificados para funcionar com o projeto AEM maior.

   `start` - executa o aplicativo Angular localmente usando um servidor Web local. Ele foi atualizado para representar o conteúdo da instância local do AEM.

   `build` - compila o aplicativo do Angular para distribuição em produção. A adição de `&& clientlib` O é responsável por copiar o SPA compilado na `ui.apps` como uma biblioteca do lado do cliente durante uma build. O módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) é usado para facilitar isso.

   Mais detalhes sobre os scripts disponíveis podem ser encontrados [aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect o arquivo `ui.frontend/clientlib.config.js`. Este arquivo de configuração é usado por [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar como gerar a biblioteca do cliente.

7. Inspect o arquivo `ui.frontend/pom.xml`. Esse arquivo transforma o `ui.frontend` pasta em um [Módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). A variável `pom.xml` o arquivo foi atualizado para usar o [front-end-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **test** e **build** o SPA durante uma compilação do Maven.

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

   `app.component.js` é o ponto de entrada do SPA. `ModelManager` AEM é fornecido pelo SDK JS do Editor de SPA. Ele é responsável por chamar e injetar o `pageModel` (o conteúdo JSON) no aplicativo.

## Adicionar um componente de cabeçalho {#header-component}

Em seguida, adicione um novo componente ao SPA e implante as alterações em uma instância de AEM local para ver a integração.

1. Abra uma nova janela de terminal e navegue até a `ui.frontend` pasta:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Instalar [CLI do Angular](https://angular.io/cli#installing-angular-cli) globalmente É usado para gerar componentes do Angular, bem como para criar e veicular o aplicativo do Angular por meio da **ng** comando.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > A versão de **@angular/cli** usado por este projeto é **9.1.7**. É recomendável manter as versões da CLI do Angular em sincronia.

3. Criar um novo `Header` executando a CLI do Angular `ng generate component` de dentro do `ui.frontend` pasta.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Isso criará um esqueleto para o novo componente de Cabeçalho do Angular em `ui.frontend/src/app/components/header`.

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

   Observe que esse exibe conteúdo estático, de modo que esse componente do Angular não requer ajustes no padrão gerado `header.component.ts`.

6. Abra o arquivo **app.component.html** em  `ui.frontend/src/app/app.component.html`. Adicione o `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Isso incluirá a `header` acima de todo o conteúdo da página.

7. Abra um novo terminal e acesse o `ui.frontend` e execute o `npm run build` comando:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navegue até a `ui.apps` pasta. Abaixo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` você deve ver os arquivos SPA compilados que foram copiados do`ui.frontend/build` pasta.

   ![Biblioteca do cliente gerada em ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

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

   Isso implantará o `ui.apps` para uma instância local em execução do AEM.

10. Abra uma guia do navegador e acesse [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Agora você deve ver o conteúdo do `Header` componente exibido no SPA.

   ![Implementação do cabeçalho inicial](assets/integrate-spa/initial-header-implementation.png)

   Etapas **7-9** são executados automaticamente ao acionar uma build Maven da raiz do projeto (ou seja, `mvn clean install -PautoInstallSinglePackage`). Agora você deve entender as noções básicas da integração entre as bibliotecas do lado do cliente SPA e AEM. Observe que ainda é possível editar e adicionar `Text` componentes no AEM, contudo, a `Header` componente não editável.

## Servidor de desenvolvimento do Webpack - Criar proxy para a API JSON {#proxy-json}

Como visto nos exercícios anteriores, executar um build e sincronizar a biblioteca do cliente com uma instância local do AEM leva alguns minutos. Isso é aceitável para o teste final, mas não é ideal para a maioria do desenvolvimento de SPA.

A [servidor de desenvolvimento do webpack](https://webpack.js.org/configuration/dev-server/) podem ser usados para desenvolver rapidamente o SPA. O SPA é orientado por um modelo JSON gerado pelo AEM. Neste exercício, o conteúdo JSON de uma instância em execução do AEM é **proxy** no servidor de desenvolvimento configurado pelo [Angular projeto](https://angular.io/guide/build).

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

   A variável [aplicativo Angular](https://angular.io/guide/build#proxying-to-a-backend-server) O fornece um mecanismo fácil para solicitações de API de proxy. Os padrões especificados em `context` são enviadas por proxy através de `localhost:4502`, o início rápido do AEM local.

2. Abra o arquivo **index.html** em `ui.frontend/src/index.html`. Esse é o arquivo HTML raiz usado pelo servidor dev.

   Observe que há uma entrada para `base href="/"`. A variável [tag base](https://angular.io/guide/deployment#the-base-tag) é essencial para o aplicativo resolver URLs relativos.

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

4. Abra uma nova guia do navegador (se ainda não estiver aberta) e acesse [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Servidor de desenvolvimento do Webpack - proxy json](assets/integrate-spa/webpack-dev-server-1.png)

   Você deve ver o mesmo conteúdo que no AEM, mas sem nenhum dos recursos de criação ativados.

5. Retorne ao IDE e crie uma nova pasta chamada `img` em `ui.frontend/src/assets`.
6. Baixe e adicione o seguinte logotipo WKND à `img` pasta:

   ![Logotipo da WKND](./assets/integrate-spa/wknd-logo-dk.png)

7. Abertura **header.component.html** em `ui.frontend/src/app/components/header/header.component.html` e incluir o logotipo:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Salvar as alterações em **header.component.html**.

8. Retorne ao navegador. Você deve ver imediatamente as alterações no aplicativo refletidas.

   ![Logotipo adicionado ao cabeçalho](assets/integrate-spa/added-logo-localhost.png)

   Você pode continuar a fazer atualizações de conteúdo no **AEM** e vê-los refletidos em **servidor de desenvolvimento do webpack**, já que estamos usando o proxy no conteúdo. Observe que as alterações de conteúdo só são visíveis no **servidor de desenvolvimento do webpack**.

9. Parar o servidor Web local com `ctrl+c` no terminal.

## Servidor de desenvolvimento Webpack - API JSON fictícia {#mock-json}

Outra abordagem para o desenvolvimento rápido é usar um arquivo JSON estático para agir como o modelo JSON. Ao &quot;zombar&quot; do JSON, removemos a dependência em uma instância local do AEM. Ele também permite que um desenvolvedor de front-end atualize o modelo JSON para testar a funcionalidade e direcionar alterações na API JSON, que seria implementada posteriormente por um desenvolvedor de back-end.

A configuração inicial do modelo JSON faz **exigir uma instância de AEM local**.

1. No navegador, navegue até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Este é o JSON exportado pelo AEM que está direcionando o aplicativo. Copie a saída JSON.

2. Retorne ao IDE para navegar até `ui.frontend/src` e adicionar novas pastas chamadas **zombarias** e **json** para corresponder à seguinte estrutura de pastas:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Crie um novo arquivo chamado **en.model.json** debaixo `ui.frontend/public/mocks/json`. Cole a saída JSON de **Etapa 1** aqui.

   ![Arquivo Json do modelo fictício](assets/integrate-spa/mock-model-json-created.png)

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

   Essa configuração de proxy regravará solicitações que começam com `/content/wknd-spa-angular/us` com `/mocks/json` e veiculam o arquivo JSON estático correspondente, por exemplo:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Abra o arquivo **angular.json**. Adicionar um novo **dev** configuração com uma atualização **ativos** matriz para fazer referência ao **zombarias** pasta criada.

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

   ![Pasta de atualização de ativos JSON Dev do Angular](assets/integrate-spa/dev-assets-update-folder.png)

   Criação de uma **dev** A configuração do garante que o **zombarias** A pasta é usada somente durante o desenvolvimento e nunca é implantada no AEM em um build de produção.

6. No **angular.json** arquivo, próxima atualização do **browserTarget** configuração para usar o novo **dev** configuração:

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

   ![Atualização de desenvolvimento da compilação JSON do Angular](assets/integrate-spa/angular-json-build-dev-update.png)

7. Abra o arquivo `ui.frontend/package.json` e adicionar um novo **início:simulado** comando para fazer referência ao **proxy.mock.conf.json** arquivo.

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

   Adicionar um novo comando facilita a alternância entre as configurações de proxy.

8. Se estiver em execução no momento, interrompa o **servidor de desenvolvimento do webpack**. Inicie o **servidor de desenvolvimento do webpack** usando o **início:simulado** script:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navegue até [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) e você deve ver o mesmo SPA, mas o conteúdo está sendo retirado do **modelo** Arquivo JSON.

9. Faça uma pequena alteração no **en.model.json** arquivo criado anteriormente. O conteúdo atualizado deve ser refletido imediatamente na variável **servidor de desenvolvimento do webpack**.

   ![atualização de json do modelo fictício](./assets/integrate-spa/webpack-mock-model.gif)

   Ser capaz de manipular o modelo JSON e ver os efeitos em um SPA ao vivo pode ajudar um desenvolvedor a entender a API do modelo JSON. Também permite que o desenvolvimento de front-end e back-end ocorra em paralelo.

## Adicionar Estilos com Sass

Em seguida, alguns estilos atualizados são adicionados ao projeto. Este projeto adicionará [Sass](https://sass-lang.com/) suporte para alguns recursos úteis, como variáveis.

1. Abra uma janela de terminal e pare o **servidor de desenvolvimento do webpack** se iniciado. De dentro do `ui.frontend` pasta digite o seguinte comando para atualizar o aplicativo Angular para processar **.scss** arquivos.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Isso atualizará o `angular.json` com uma nova entrada na parte inferior do arquivo:

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
4. Criar um novo arquivo abaixo de `ui.frontend/src/styles` nomeado `_variables.scss` e preencha-o com as seguintes variáveis:

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

5. Renomeie a extensão do arquivo **estilos.css** em `ui.frontend/src/styles.css` para **estilos.scss**. Substitua o conteúdo pelo seguinte:

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

6. Atualizar **angular.json** e renomear todas as referências a **style.css** com **estilos.scss**. Deve haver 3 referências.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Atualizar estilos de cabeçalho

Em seguida, adicione alguns estilos específicos da marca à **Cabeçalho** componente usando Sass.

1. Inicie o **servidor de desenvolvimento do webpack** para ver os estilos atualizados em tempo real:

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

3. Atualizar **header.component.ts** para referência **header.component.scss**:

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

4. Retorne ao navegador e à janela **servidor de desenvolvimento do webpack**:

   ![Cabeçalho estilizado - servidor de desenvolvimento de webpack](assets/integrate-spa/styled-header.png)

   Agora você deve ver os estilos atualizados adicionados à **Cabeçalho** componente.

## Implantar atualizações do SPA no AEM

As alterações feitas à **Cabeçalho** atualmente só são visíveis através do **servidor de desenvolvimento do webpack**. Implante o AEM atualizado para SPA para ver as alterações.

1. Interrompa o **servidor de desenvolvimento do webpack**.
2. Navegue até a raiz do projeto `/aem-guides-wknd-spa` e implante o projeto no AEM usando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Você deve ver o **Cabeçalho** com logotipo e estilos aplicados:

   ![Cabeçalho atualizado no AEM](assets/integrate-spa/final-header-component.png)

   Agora que o SPA atualizado está no AEM, a criação pode continuar.

## Parabéns. {#congratulations}

Parabéns, você atualizou o SPA e explorou a integração com o AEM! Agora você conhece duas abordagens diferentes para desenvolver o SPA contra a API do modelo JSON AEM usando um **servidor de desenvolvimento do webpack**.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ou verifique o código localmente alternando para a ramificação `Angular/integrate-spa-solution`.

### Próximas etapas {#next-steps}

[Mapear componentes do SPA para componentes do AEM](map-components.md) - Saiba como mapear componentes do Angular para componentes do Adobe Experience Manager (AEM) com o SDK JS do editor do AEM SPA. O mapeamento de componentes permite que os autores façam atualizações dinâmicas nos componentes do SPA no editor SPA AEM, de forma semelhante à criação tradicional do AEM.
