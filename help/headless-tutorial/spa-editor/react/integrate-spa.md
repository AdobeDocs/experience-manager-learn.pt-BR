---
title: Integrar um SPA | Introdução ao AEM SPA Editor e React
description: Entenda como o código-fonte de um Aplicativo de página única (SPA) escrito no React pode ser integrado a um projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas de front-end modernas, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente o SPA em relação à API do modelo JSON do AEM.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
duration: 409
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 0%

---

# Integrar o SPA {#developer-workflow}

{{spa-editor-deprecation}}

Entenda como o código-fonte de um Aplicativo de página única (SPA) escrito no React pode ser integrado a um projeto do Adobe Experience Manager (AEM). Saiba como usar ferramentas de front-end modernas, como um servidor de desenvolvimento de webpack, para desenvolver rapidamente o SPA em relação à API do modelo JSON do AEM.

## Objetivo

1. Entenda como o projeto de SPA é integrado ao AEM com bibliotecas do lado do cliente.
2. Saiba como usar um servidor de desenvolvimento de webpack para desenvolvimento front-end dedicado.
3. Explore o uso de um **proxy** e de um arquivo estático **mock** para desenvolvimento em relação à API de modelo JSON do AEM.

## O que você vai criar

Neste capítulo, você fará várias pequenas alterações no SPA para entender como ele é integrado ao AEM.
Este capítulo adicionará um componente `Header` simples ao SPA. No processo de criação deste componente **estático** `Header`, várias abordagens para o desenvolvimento de SPA do AEM são usadas.

![Novo cabeçalho no AEM](./assets/integrate-spa/final-header-component.png)

*O SPA foi estendido para adicionar um componente `Header` estático*

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Este capítulo é uma continuação do capítulo [Criar projeto](create-project.md). No entanto, basta seguir um projeto AEM habilitado para SPA para que você possa trabalhar.

## Abordagem de integração {#integration-approach}

Dois módulos foram criados como parte do projeto AEM: `ui.apps` e `ui.frontend`.

O módulo `ui.frontend` é um projeto [webpack](https://webpack.js.org/) que contém todo o código-fonte do SPA. A maioria do desenvolvimento e teste de SPA é feita no projeto webpack. Quando uma build de produção é acionada, o SPA é criado e compilado usando o webpack. Os artefatos compilados (CSS e Javascript) são copiados no módulo `ui.apps` que é então implantado no tempo de execução do AEM.

![Arquitetura de alto nível ui.frontend](assets/integrate-spa/ui-frontend-architecture.png)

*Uma descrição detalhada da integração de SPA.*

Informações adicionais sobre a compilação de front-end podem ser [encontradas aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspecione a integração de SPA {#inspect-spa-integration}

Em seguida, inspecione o módulo `ui.frontend` para entender o SPA que foi gerado automaticamente pelo [Arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. No IDE de sua escolha, abra o projeto do AEM. Este tutorial usará o [Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - Projeto AEM WKND SPA](./assets/integrate-spa/vscode-ide-openproject.png)

1. Expanda e inspecione a pasta `ui.frontend`. Abrir o arquivo `ui.frontend/package.json`

1. Em `dependencies` você deve ver vários relacionados a `react` incluindo `react-scripts`

   O `ui.frontend` é um aplicativo React com base no [Criar aplicativo React](https://create-react-app.dev/) ou CRA para abreviar. A versão `react-scripts` indica qual versão do CRA é usada.

1. Há também várias dependências com o prefixo `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   Os módulos acima compõem o [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) e fornecem a funcionalidade para tornar possível mapear Componentes SPA para Componentes AEM.

   Também estão incluídos [Componentes do AEM WCM - Implementação do React Core](https://github.com/adobe/aem-react-core-wcm-components-base) e [Componentes do AEM WCM - Editor de spa - Implementação do React Core](https://github.com/adobe/aem-react-core-wcm-components-spa). Esses são um conjunto de componentes reutilizáveis da interface do usuário que mapeiam para componentes prontos para uso do AEM. Elas foram projetadas para serem usadas como estão e estilizadas para atender às necessidades do seu projeto.

1. No arquivo `package.json` há vários `scripts` definidos:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Estes são os scripts de compilação padrão [disponibilizados](https://create-react-app.dev/docs/available-scripts) pela opção Criar Aplicativo React.

   A única diferença é a adição de `&& clientlib` ao script `build`. Essa instrução extra é responsável por copiar o SPA compilado no módulo `ui.apps` como uma biblioteca do lado do cliente durante uma compilação.

   O módulo npm [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) é usado para facilitar isso.

1. Inspecione o arquivo `ui.frontend/clientlib.config.js`. Este arquivo de configuração é usado por [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) para determinar como gerar a biblioteca do cliente.

1. Inspecione o arquivo `ui.frontend/pom.xml`. Este arquivo transforma a pasta `ui.frontend` em um [módulo Maven](https://maven.apache.org/guides/mini/guide-multiple-modules.html). O arquivo `pom.xml` foi atualizado para usar o [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) para **testar** e **compilar** o SPA durante uma compilação Maven.

1. Inspecionar o arquivo `index.js` em `ui.frontend/src/index.js`:

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

   `index.js` é o ponto de entrada do SPA. `ModelManager` é fornecido pelo AEM SPA Editor JS SDK. Ele é responsável por chamar e injetar o `pageModel` (o conteúdo JSON) no aplicativo.

1. Inspecione o arquivo `import-components.js` em `ui.frontend/src/components/import-components.js`. Este arquivo importa os **Componentes principais do React** prontos para uso e os disponibiliza para o projeto. Inspecionaremos o mapeamento do conteúdo do AEM para componentes SPA no próximo capítulo.

## Adicionar um componente SPA estático {#static-spa-component}

Em seguida, adicione um novo componente ao SPA e implante as alterações em uma instância local do AEM. É uma alteração simples, apenas para ilustrar como o SPA é atualizado.

1. No módulo `ui.frontend`, abaixo de `ui.frontend/src/components`, crie uma nova pasta chamada `Header`.
1. Crie um arquivo chamado `Header.js` abaixo da pasta `Header`.

   ![Pasta e arquivo de cabeçalho](assets/create-project/header-folder-js.png)

1. Popular `Header.js` com o seguinte:

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

   Acima está um componente React padrão que resultará em uma sequência de caracteres de texto estática.

1. Abra o arquivo `ui.frontend/src/App.js`. Este é o ponto de entrada do aplicativo.
1. Faça as seguintes atualizações em `App.js` para incluir o `Header` estático:

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

1. Abra um novo terminal, navegue até a pasta `ui.frontend` e execute o comando `npm run build`:

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

1. Navegue até a pasta `ui.apps`. Abaixo de `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` você deve ver que os arquivos SPA compilados foram copiados da pasta `ui.frontend/build`.

   ![Biblioteca do cliente gerada em ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Retorne ao terminal e navegue até a pasta `ui.apps`. Execute o seguinte comando Maven:

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

   Isso implantará o pacote `ui.apps` em uma instância do AEM em execução local.

1. Abra uma guia do navegador e navegue até [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Agora você deve ver o conteúdo do componente `Header` sendo exibido no SPA.

   ![Implementação de cabeçalho inicial](./assets/integrate-spa/initial-header-implementation.png)

   As etapas acima são executadas automaticamente ao acionar uma compilação Maven da raiz do projeto (ou seja, `mvn clean install -PautoInstallSinglePackage`). Agora você deve entender as noções básicas da integração entre o SPA e as bibliotecas do lado do cliente do AEM. Observe que você ainda pode editar e adicionar `Text` componentes no AEM abaixo do componente estático `Header`.

## Servidor de desenvolvimento do Webpack - Criar proxy para a API JSON {#proxy-json}

Como visto nos exercícios anteriores, executar um build e sincronizar a biblioteca do cliente com uma instância local do AEM leva alguns minutos. Isso é aceitável para o teste final, mas não é ideal para a maioria do desenvolvimento de SPA.

Um [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) pode ser usado para desenvolver rapidamente o SPA. O SPA é orientado por um modelo JSON gerado pelo AEM. Neste exercício, o conteúdo JSON de uma instância do AEM em execução é **encaminhado por proxy** para o servidor de desenvolvimento.

1. Retorne ao IDE e abra o arquivo `ui.frontend/package.json`.

   Procure uma linha como a seguinte:

   ```json
   "proxy": "http://localhost:4502",
   ```

   O [Criar aplicativo React](https://create-react-app.dev/docs/proxying-api-requests-in-development) fornece um mecanismo fácil para solicitações de API de proxy. Todas as solicitações desconhecidas são encaminhadas por proxy através de `localhost:4502`, a inicialização rápida do AEM local.

1. Abra uma janela de terminal e navegue até a pasta `ui.frontend`. Execute o comando `npm start`:

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

1. Abra uma nova guia do navegador (se ainda não estiver aberta) e navegue até [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Servidor de desenvolvimento do Webpack - proxy json](./assets/integrate-spa/webpack-dev-server-1.png)

   Você deve ver o mesmo conteúdo que no AEM, mas sem nenhum dos recursos de criação ativados.

   >[!NOTE]
   >
   > Devido aos requisitos de segurança do AEM, será necessário fazer logon na instância local do AEM (http://localhost:4502) no mesmo navegador, mas em uma guia diferente.

1. Retorne ao IDE e crie um arquivo chamado `Header.css` na pasta `src/components/Header`.
1. Preencha o `Header.css` com o seguinte:

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![IDE de VSCode](assets/integrate-spa/header-css-update.png)

1. Reabra `Header.js` e adicione a seguinte linha à referência `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Salve as alterações.

1. Navegue até [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) para ver as alterações de estilo refletidas automaticamente.

1. Abra o arquivo `Page.css` em `ui.frontend/src/components/Page`. Faça as seguintes alterações para corrigir o preenchimento:

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Retorne ao navegador em [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). Você deve ver imediatamente as alterações no aplicativo refletidas.

   ![Estilo adicionado ao cabeçalho](assets/integrate-spa/added-logo-localhost.png)

   Você pode continuar a fazer atualizações de conteúdo no AEM e vê-las refletidas no **webpack-dev-server**, já que estamos usando o proxy para o conteúdo.

1. Interrompa o servidor de desenvolvimento do webpack com `ctrl+c` no terminal.

## Implantar atualizações de SPA no AEM

As alterações feitas no `Header` atualmente só são visíveis por meio do **webpack-dev-server**. Implante o SPA atualizado no AEM para ver as alterações.

1. Navegue até a raiz do projeto (`aem-guides-wknd-spa`) e implante o projeto no AEM usando Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Você deve ver os `Header` e estilos atualizados aplicados.

   ![Cabeçalho atualizado no AEM](assets/integrate-spa/final-header-component.png)

   Agora que o SPA atualizado está no AEM, a criação pode continuar.

## Parabéns! {#congratulations}

Parabéns, você atualizou o SPA e explorou a integração com o AEM! Agora você sabe como desenvolver o SPA em relação à API do modelo JSON do AEM usando um **webpack-dev-server**.

### Próximas etapas {#next-steps}

[Mapear componentes de SPA para componentes do AEM](map-components.md) - Saiba como mapear componentes do React para componentes do Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas em componentes SPA no Editor SPA do AEM, de modo semelhante à criação tradicional no AEM.

## (Bônus) Servidor de desenvolvimento Webpack - API JSON simulada {#mock-json}

Outra abordagem para o desenvolvimento rápido é usar um arquivo JSON estático para agir como o modelo JSON. Ao &quot;zombar&quot; o JSON, removemos a dependência em uma instância do AEM local. Ele também permite que um desenvolvedor de front-end atualize o modelo JSON para testar a funcionalidade e direcionar alterações na API JSON, que seria implementada posteriormente por um desenvolvedor de back-end.

A configuração inicial do JSON de modelo **requer uma instância do AEM local**.

1. Retorne ao IDE, navegue até `ui.frontend/public` e adicione uma nova pasta chamada `mock-content`.
1. Crie um novo arquivo com o nome `mock.model.json` debaixo de `ui.frontend/public/mock-content`.
1. No navegador, navegue até [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Este é o JSON exportado pelo AEM que está direcionando o aplicativo. Copie a saída JSON.

1. Cole a saída JSON da etapa anterior no arquivo `mock.model.json`.

   ![Arquivo Json de Modelo Fictício](./assets/integrate-spa/mock-model-json-created.png)

1. Abra o arquivo `index.html` em `ui.frontend/public/index.html`. Atualize a propriedade de metadados do modelo de página do AEM para apontar para uma variável `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Usar uma variável para o valor de `cq:pagemodel_root_url` facilitará a alternância entre o modelo proxy e o modelo json fictício.

1. Abra o arquivo `ui.frontend/.env.development` e faça as seguintes atualizações para comentar o valor anterior de `REACT_APP_PAGE_MODEL_PATH` e `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. Se estiver em execução, pare o **webpack-dev-server**. Inicie o **webpack-dev-server** do terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Navegue até [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) e você deverá ver o SPA com o mesmo conteúdo usado no **proxy** json.

1. Faça uma pequena alteração no arquivo `mock.model.json` criado anteriormente. Você deve ver o conteúdo atualizado imediatamente refletido no **webpack-dev-server**.

   ![atualização de json do modelo fictício](./assets/integrate-spa/webpack-mock-model.gif)

Ser capaz de manipular o modelo JSON e ver os efeitos em um SPA em tempo real pode ajudar um desenvolvedor a entender a API do modelo JSON. Também permite que o desenvolvimento de front-end e back-end ocorra em paralelo.

Agora é possível alternar onde consumir o conteúdo JSON, alternando as entradas no arquivo `env.development`:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
