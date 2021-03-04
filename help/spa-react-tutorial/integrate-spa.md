---
title: Integrar um SPA | Introdução ao AEM SPA Editor e React
description: Entenda como o código-fonte de um Aplicativo de página única (SPA) gravado no React pode ser integrado a um projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas de front-end modernas, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente o SPA em relação à API de modelo JSON do AEM.
sub-product: sites
feature: Editor SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2109'
ht-degree: 1%

---


# Integrar um SPA {#integrate-spa}

Entenda como o código-fonte de um Aplicativo de página única (SPA) gravado no React pode ser integrado a um projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas de front-end modernas, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente o SPA em relação à API de modelo JSON do AEM.

## Objetivo

1. Entenda como o projeto SPA é integrado ao AEM com bibliotecas do lado do cliente.
2. Saiba como usar um servidor de desenvolvimento de webpack para desenvolvimento front-end dedicado.
3. Explore o uso de um arquivo **proxy** e **mock** estático para desenvolvimento em relação à API de modelo JSON do AEM

## O que você vai criar

Este capítulo adicionará um componente simples `Header` ao SPA. No processo de criação desse componente estático `Header`, várias abordagens para o desenvolvimento do AEM SPA serão usadas.

![Novo cabeçalho no AEM](./assets/integrate-spa/final-header-component.png)

*O SPA é estendido para adicionar um  `Header` componente estático*

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Implante a base de código em uma instância do AEM local usando o Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualizar o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ou verificar o código localmente ao alternar para a ramificação `React/integrate-spa-solution`.

## Abordagem de integração {#integration-approach}

Dois módulos foram criados como parte do projeto AEM: `ui.apps` e `ui.frontend`.

O módulo `ui.frontend` é um projeto [webpack](https://webpack.js.org/) que contém todo o código-fonte SPA. A maioria do desenvolvimento e teste de SPA será feito no projeto do webpack. Quando uma build de produção é acionada, o SPA é criado e compilado usando o webpack. Os artefatos compilados (CSS e Javascript) são copiados no módulo `ui.apps` que é então implantado no tempo de execução do AEM.

![arquitetura de alto nível ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Uma descrição de alto nível da integração do SPA.*

Informações adicionais sobre a build do Front-end podem ser [encontradas aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspecionar a integração do SPA {#inspect-spa-integration}

Em seguida, inspecione o módulo `ui.frontend` para entender o SPA que foi gerado automaticamente pelo [arquétipo de projeto AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. No IDE de sua escolha, abra o Projeto AEM para o SPA da WKND. Este tutorial usará o [Visual Studio Code IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - Projeto SPA WKND do AEM](./assets/integrate-spa/vscode-ide-openproject.png)

2. Expanda e inspecione a pasta `ui.frontend`. Abra o arquivo `ui.frontend/package.json`

3. Em `dependencies` você deve ver vários relacionados a `react` incluindo `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   O `ui.frontend` é um aplicativo React com base no [Create React App](https://create-react-app.dev/) ou no CRA para encurtar. A versão `react-scripts` indica qual versão do CRA é usada.

4. Há também três dependências com o prefixo `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   Os módulos acima compõem o [SDK JS do Editor SPA do AEM SPA](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) e fornecem a funcionalidade para possibilitar o mapeamento de componentes SPA para componentes do AEM.

5. No arquivo `package.json` existem vários `scripts` definidos:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Esses são scripts de criação padrão [disponíveis](https://create-react-app.dev/docs/available-scripts) pelo aplicativo Criar reação.

   A única diferença é a adição de `&& clientlib` ao script `build`. Essa instrução extra é responsável pela cópia do SPA compilado no módulo `ui.apps` como uma biblioteca do lado do cliente durante uma build.

   O módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) é usado para facilitar isso.

6. Inspecione o arquivo `ui.frontend/clientlib.config.js`. Esse arquivo de configuração é usado pelo [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar como gerar a biblioteca do cliente.

7. Inspecione o arquivo `ui.frontend/pom.xml`. Esse arquivo transforma a pasta `ui.frontend` em um [módulo Maven](http://maven.apache.org/guides/mini/guide-multiple-modules.html). O arquivo `pom.xml` foi atualizado para usar o [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **testar** e **criar** o SPA durante uma compilação Maven.

8. Inspecione o arquivo `index.js` em `ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` é o ponto de entrada do SPA. `ModelManager` é fornecido pelo SDK JS do Editor do SPA do AEM. É responsável por chamar e injetar o `pageModel` (o conteúdo JSON) no aplicativo.

## Adicionar um componente Cabeçalho {#header-component}

Em seguida, adicione um novo componente ao SPA e implante as alterações em uma instância do AEM local.

1. No módulo `ui.frontend`, abaixo de `ui.frontend/src/components` crie uma nova pasta chamada `Header`.
2. Crie um arquivo com o nome `Header.js` abaixo da pasta `Header`.

   ![Pasta e arquivo do cabeçalho](assets/integrate-spa/header-folder-js.png)

3. Preencha `Header.js` com o seguinte:

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   Acima é um componente React padrão que resultará em uma sequência de texto estática.

4. Abra o arquivo `ui.frontend/src/App.js`. Este é o ponto de entrada do aplicativo.
5. Faça as seguintes atualizações em `App.js` para incluir o `Header` estático:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

6. Abra um novo terminal e navegue até a pasta `ui.frontend` e execute o comando `npm run build`:

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

7. Navegue até a pasta `ui.apps`. Abaixo de `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` você deve ver que os arquivos SPA compilados foram copiados da pasta`ui.frontend/build`.

   ![Biblioteca de clientes gerada em ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Retorne ao terminal e navegue até a pasta `ui.apps`. Execute o seguinte comando Maven:

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

9. Abra uma guia do navegador e navegue até [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Agora você deve ver o conteúdo do componente `Header` sendo exibido no SPA.

   ![Implementação do cabeçalho inicial](./assets/integrate-spa/initial-header-implementation.png)

   As etapas 6 a 8 são executadas automaticamente ao acionar uma build Maven da raiz do projeto (ou seja `mvn clean install -PautoInstallSinglePackage`). Agora você deve entender as noções básicas da integração entre o SPA e as bibliotecas do lado do cliente do AEM. Observe que você ainda pode editar e adicionar `Text` componentes no AEM abaixo do componente estático `Header`.

## Servidor de Desenvolvimento de Webpack - Proxy da API JSON {#proxy-json}

Como visto nos exercícios anteriores, a execução de uma build e sincronização da biblioteca do cliente para uma instância local do AEM demora alguns minutos. Isso é aceitável para testes finais, mas não é ideal para a maioria do desenvolvimento de SPA.

Um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pode ser usado para desenvolver rapidamente o SPA. O SPA é orientado por um modelo JSON gerado pelo AEM. Neste exercício, o conteúdo JSON de uma instância em execução do AEM será **proxied** no servidor de desenvolvimento.

1. Retorne ao IDE e abra o arquivo `ui.frontend/package.json`.

   Procure uma linha como a seguinte:

   ```json
   "proxy": "http://localhost:4502",
   ```

   O [Criar aplicativo React](https://create-react-app.dev/docs/proxying-api-requests-in-development) fornece um mecanismo fácil para solicitações de API de proxy. Todas as solicitações desconhecidas serão enviadas por proxy por meio de `localhost:4502`, o início rápido do AEM local.

2. Abra uma janela de terminal e navegue até a pasta `ui.frontend`. Execute o comando `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

3. Abra uma nova guia do navegador (se ainda não estiver aberta) e navegue até [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Servidor de desenvolvimento do Webpack - json proxy](./assets/integrate-spa/webpack-dev-server-1.png)

   Você deve ver o mesmo conteúdo que no AEM, mas sem nenhum dos recursos de criação ativados.

   >[!NOTE]
   >
   > Devido aos requisitos de segurança do AEM, você precisará estar conectado à instância do AEM local (http://localhost:4502) no mesmo navegador, mas em uma guia diferente.

4. Retorne ao IDE e crie uma nova pasta chamada `media` em `ui.frontend/src/media`.
5. Baixe e adicione o seguinte logotipo WKND à pasta `media`:

   ![Logotipo da WKND](./assets/integrate-spa/wknd-logo-dk.png)

6. Abra `Header.js` em `ui.frontend/src/components/Header/Header.js` e importe o logotipo:

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Faça as seguintes atualizações em `Header.js` para incluir o logotipo como parte do cabeçalho:

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   Salve as alterações em `Header.js`.

8. Retorne ao navegador em [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Você deve ver imediatamente as alterações no aplicativo refletidas.

   ![Logotipo adicionado ao cabeçalho](./assets/integrate-spa/added-logo-localhost.png)

   Você pode continuar fazendo atualizações de conteúdo no AEM e vê-las refletidas em **webpack-dev-server**, já que estamos enviando o conteúdo por proxy.

9. Pare o servidor de desenvolvimento de webpack com `ctrl+c` no terminal.

## Servidor de Desenvolvimento de Webpack - API JSON Mock {#mock-json}

Outra abordagem para o desenvolvimento rápido é usar um arquivo JSON estático para agir como o modelo JSON. Ao &quot;zombar&quot; o JSON, removemos a dependência de uma instância do AEM local. Ele também permite que um desenvolvedor de front-end atualize o modelo JSON para testar a funcionalidade e direcionar alterações para a API JSON que seriam implementadas posteriormente por um desenvolvedor de back-end.

A configuração inicial do modelo JSON **requer uma instância do AEM local**.

1. No navegador, navegue até [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Este é o JSON exportado pelo AEM que está liderando o aplicativo. Copie a saída JSON.

2. Volte para o IDE e navegue até `ui.frontend/public` e adicione uma nova pasta chamada `mock-content`.
3. Crie um novo arquivo com o nome `mock.model.json` debaixo de `ui.frontend/public/mock-content`. Cole a saída JSON de **Etapa 1** aqui.

   ![Arquivo Json do Modelo de Mock](./assets/integrate-spa/mock-model-json-created.png)

4. Abra o arquivo `index.html` em `ui.frontend/public/index.html`. Atualize a propriedade de metadados do modelo de página do AEM para apontar para uma variável `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Usar uma variável para o valor do `cq:pagemodel_root_url` facilitará a alternância entre o proxy e o modelo json de modelo.

5. Abra o arquivo `ui.frontend/.env.development` e faça as seguintes atualizações para comentar o valor anterior de `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. Se estiver em execução no momento, pare o **webpack-dev-server**. Inicie o **webpack-dev-server** do terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Navegue até [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) e você deverá ver o SPA com o mesmo conteúdo usado no json **proxy**.

7. Faça uma pequena alteração no arquivo `mock.model.json` criado anteriormente. Você deve ver o conteúdo atualizado imediatamente refletido no **webpack-dev-server**.

   ![modelo de modelo json atualizado](./assets/integrate-spa/webpack-mock-model.gif)

Ser capaz de manipular o modelo JSON e ver os efeitos em um SPA ao vivo pode ajudar um desenvolvedor a entender a API do modelo JSON. Também permite que tanto o desenvolvimento front-end quanto o back-end aconteçam em paralelo.

Agora você pode alternar onde consumir o conteúdo JSON, alternando as entradas no arquivo `env.development`:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Adicionar estilos com o Sass

Uma prática recomendada do React é manter cada componente modular e autocontido. Uma recomendação geral é evitar o reuso do mesmo nome de classe CSS entre componentes, o que torna o uso de pré-processadores menos potente. Este projeto usará [Sass](https://sass-lang.com/) para alguns recursos úteis como variáveis. Este projeto também seguirá vagamente [Convenções de nomenclatura CSS SUIT](https://github.com/suitcss/suit/blob/master/doc/components.md). O SUIT é uma variação da notação BEM, o modificador de elemento de bloco, usada para criar regras CSS consistentes.

1. Abra uma janela de terminal e pare o **webpack-dev-server** se ele tiver sido iniciado. Dentro da pasta `ui.frontend`, digite o seguinte comando para [instalar Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Instale `sass` como uma dependência de peer:

   ```shell
   $ npm install sass --save
   ```

2. Instale `normalize-scss` para normalizar os estilos nos navegadores:

   ```shell
   $ npm install normalize-scss
   ```

3. Inicie o **webpack-dev-server** para que possamos ver os estilos sendo atualizados em tempo real:

   ```shell
   $ npm start
   ```

   Use a abordagem Proxy of Mock para manipular a API do modelo JSON.

4. Retorne ao IDE e, abaixo de `ui.frontend/src` crie uma nova pasta chamada `styles`.
5. Crie um novo arquivo sob `ui.frontend/src/styles` chamado `_variables.scss` e preencha com as seguintes variáveis:

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
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. Renomeie a extensão do arquivo `index.css` em `ui.frontend/src/index.css` para **`index.scss`**. Substitua o conteúdo pelo seguinte:

   ```scss
   /* index.scss * /
   
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
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. Atualize `ui.frontend/src/index.js` para incluir o `index.scss` renomeado:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Crie um novo arquivo com o nome `Header.scss` debaixo de `ui.frontend/src/components/Header`. Preencha o arquivo com o seguinte:

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. Inclua `Header.scss` atualizando `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Retorne ao navegador e ao **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Cabeçalho estilizado - servidor de desenvolvimento de webpack](./assets/integrate-spa/styled-header.png)

   Agora, você deve ver os estilos atualizados adicionados ao componente `Header`.

## Implantar atualizações de SPA no AEM

As alterações feitas no `Header` atualmente são visíveis apenas por meio do **webpack-dev-server**. Implante o SPA atualizado no AEM para ver as alterações.

1. Navegue até a raiz do projeto (`aem-guides-wknd-spa`) e implante o projeto no AEM usando o Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Você deve ver o `Header` atualizado com o logotipo e os estilos aplicados.

   ![Cabeçalho atualizado no AEM](./assets/integrate-spa/final-header-component.png)

   Agora que o SPA atualizado está no AEM, a criação pode continuar.

## Solução de problemas de erro do nó

Durante o desenvolvimento, você pode encontrar o seguinte erro:

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Isso pode ocorrer quando a versão local de **Node.js** e **npm** forem diferentes do que é usado pelo [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin). A execução do comando `npm rebuild node-sass` pode corrigir temporariamente o problema ou remover a pasta `ui.frontend/node_modules` e reinstalar.

Há também algumas formas de resolver esta questão de forma mais permanente.

* Certifique-se de que a versão local de npm e Node.js corresponda às versões usadas pelo [Compilação Maven](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* Adicione a seguinte etapa de execução à etapa `ui.frontend/pom.xml` antes da etapa `npm run build`:

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## Parabéns! {#congratulations}

Parabéns, você atualizou o SPA e explorou a integração com o AEM! Agora você sabe de duas abordagens diferentes para desenvolver o SPA em relação à API do modelo JSON do AEM usando um **webpack-dev-server**.

Você sempre pode visualizar o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ou verificar o código localmente ao alternar para a ramificação `React/integrate-spa-solution`.

### Próximas etapas {#next-steps}

[Mapear componentes de SPA para componentes do AEM](map-components.md)  - saiba como mapear componentes do React para componentes do Adobe Experience Manager (AEM) com o SDK JS do AEM SPA Editor. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas em componentes do SPA no Editor SPA do AEM, de forma semelhante à criação tradicional do AEM.
