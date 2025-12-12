---
title: 'Bootstrap: o SPA remoto para o editor de SPA'
description: Saiba como inicializar um SPA remoto para compatibilidade com o AEM SPA Editor.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 2%

---

# Bootstrap: o SPA remoto para o editor de SPA

{{spa-editor-deprecation}}

Para que as áreas editáveis possam ser adicionadas ao SPA remoto, ele deve ser inicializado com o AEM SPA Editor JavaScript SDK e algumas outras configurações.

## Instalar dependências npm do AEM SPA Editor JS SDK

Primeiro, analise as dependências de SPA npm do AEM para o projeto React e instale-as.

* [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : fornece a API para recuperar conteúdo do AEM.
* [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : fornece a API que mapeia o conteúdo AEM para componentes SPA.
* [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : fornece uma API para a compilação de componentes de SPA personalizados e fornece implementações de uso comum, como o componente React `AEMPage`.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## Revisar variáveis de ambiente SPA

Várias variáveis de ambiente devem ser expostas ao SPA remoto para que ele saiba como interagir com o AEM.

* Abrir projeto SPA Remoto em `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` no IDE
* Abra o arquivo `.env.development`
* No arquivo, preste atenção específica às chaves e atualize conforme necessário:

  ```
  REACT_APP_HOST_URI=http://localhost:4502
  
  REACT_APP_USE_PROXY=true
  
  REACT_APP_AUTH_METHOD=basic 
  
  REACT_APP_BASIC_AUTH_USER=admin
  REACT_APP_BASIC_AUTH_PASS=admin
  ```

  ![Variáveis de Ambiente de SPA Remoto](./assets/spa-bootstrap/env-variables.png)

  *Lembre-se de que as variáveis de ambiente personalizadas no React devem receber o prefixo `REACT_APP_`.*

   * `REACT_APP_HOST_URI`: o esquema e o host do serviço AEM ao qual o SPA Remoto se conecta.
      * Esse valor é alterado com base no tipo de serviço do AEM (local, Desenvolvimento, Preparo ou Produção) e do AEM (Autor versus Publicação)
   * `REACT_APP_USE_PROXY`: isso evita problemas do CORS durante o desenvolvimento, informando ao servidor de desenvolvimento do react para solicitações de proxy AEM, como `/content, /graphql, .model.json` usando o módulo `http-proxy-middleware`.
   * `REACT_APP_AUTH_METHOD`: método de autenticação para solicitações atendidas pelo AEM, as opções são &quot;service-token&quot;, &quot;dev-token&quot;, &quot;basic&quot; ou deixe em branco para caso de uso sem autenticação
      * Obrigatório para uso com o AEM Author
      * Possivelmente necessário para uso com o AEM Publish (se o conteúdo estiver protegido)
      * O desenvolvimento no AEM SDK é compatível com contas locais via Autenticação básica. Este é o método usado neste tutorial.
      * Ao integrar com o AEM as a Cloud Service, use [tokens de acesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   * `REACT_APP_BASIC_AUTH_USER`: o __nome de usuário__ do AEM pelo SPA para autenticar ao recuperar o conteúdo do AEM.
   * `REACT_APP_BASIC_AUTH_PASS`: a __senha__ do AEM pelo SPA para autenticar ao recuperar o conteúdo do AEM.

## Integrar a API ModelManager

Com as dependências npm de SPA do AEM disponíveis para o aplicativo, inicialize o `ModelManager` do AEM no `index.js` do projeto antes que `ReactDOM.render(...)` seja chamado.

O [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) é responsável pela conexão com o AEM para recuperar o conteúdo editável.

1. Abra o projeto do SPA remoto no IDE
1. Abra o arquivo `src/index.js`
1. Adicionar importação `ModelManager` e inicializá-la antes da invocação `root.render(..)`,

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

O arquivo `src/index.js` deve ser semelhante a:

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Configurar um proxy de SPA interno

Ao criar um SPA editável, é melhor configurar um [proxy interno no SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), que esteja configurado para rotear as solicitações apropriadas para o AEM. Isso é feito usando o módulo [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm, que já está instalado pelo aplicativo WKND GraphQL base.

1. Abra o projeto do SPA remoto no IDE
1. Abrir o arquivo em `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Atualize o arquivo com o seguinte código:

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') ||
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   O arquivo `setupProxy.spa-editor.auth.basic.js` deve ser semelhante a:

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Essa configuração de proxy faz duas coisas principais:

   1. Solicitações específicas de proxy feitas ao SPA (`http://localhost:3000`) para o AEM `http://localhost:4502`
      * Somente solicitações de proxy cujos caminhos correspondem a padrões que indicam que devem ser atendidos pelo AEM, conforme definido em `toAEM(path, req)`.
      * Ele substitui caminhos de SPA para suas páginas de AEM de contrapartida, conforme definido em `pathRewriteToAEM(path, req)`
   1. Ele adiciona cabeçalhos CORS a todas as solicitações para permitir acesso ao conteúdo AEM, conforme definido por `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      * Se isso não for adicionado, ocorrerão erros CORS ao carregar o conteúdo do AEM no SPA.

1. Abra o arquivo `src/setupProxy.js`
1. Revise a linha que aponta para o arquivo de configuração de proxy `setupProxy.spa-editor.auth.basic`:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Observe que qualquer alteração no `src/setupProxy.js` ou em seus arquivos referenciados requer uma reinicialização do SPA.

## Recurso SPA estático

Os recursos de SPA estáticos, como o Logotipo WKND e o Carregamento de gráficos, precisam ter seus URLs src atualizados para forçá-los a serem carregados do host do SPA remoto. Se relativo, quando o SPA é carregado no Editor de SPA para criação, esses URLs assumem o padrão de usar o host do AEM, em vez do SPA, resultando em 404 solicitações, conforme ilustrado na imagem abaixo.

![Recursos estáticos desfeitos](./assets/spa-bootstrap/broken-static-resource.png)

Para resolver esse problema, faça com que um recurso estático hospedado pelo SPA remoto use caminhos absolutos que incluam a origem do SPA remoto.

1. Abra o projeto SPA no IDE
1. Abra o arquivo de variáveis de ambiente `src/.env.development` do SPA e adicione uma variável para o URI público do SPA:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Ao implantar no AEM as a Cloud Service, você precisa fazer o mesmo para os `.env` arquivos correspondentes._

1. Abra o arquivo `src/App.js`
1. Importe o URI público de SPA das variáveis de ambiente de SPA

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Prefixe o logotipo WKND `<img src=.../>` com `REACT_APP_PUBLIC_URI` para forçar a resolução contra o SPA.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Faça o mesmo para carregar a imagem em `src/components/Loading.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. E para as __duas instâncias__ do botão Voltar em `src/components/AdventureDetails.js`

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Os arquivos `App.js`, `Loading.js` e `AdventureDetails.js` devem ter a seguinte aparência:

![Recursos estáticos](./assets/spa-bootstrap/static-resources.png)

## Grade responsiva do AEM

Para oferecer suporte ao modo de layout do Editor de SPA para áreas editáveis no SPA, devemos integrar o CSS de grade responsiva do AEM no SPA. Não se preocupe: esse sistema de grade é aplicável apenas aos contêineres editáveis e você pode usar o sistema de grade de sua escolha para direcionar o layout do restante do SPA.

Adicione os arquivos SCSS da Grade responsiva do AEM ao SPA.

1. Abra o projeto SPA no IDE
1. Baixe e copie os dois arquivos a seguir em `src/styles`
   * [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      * O gerador de SCSS da grade responsiva do AEM
   * [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      * Invoca `_grid.scss` usando os pontos de interrupção específicos do SPA (desktop e dispositivos móveis) e as colunas (12).
1. Abrir `src/App.scss` e importar `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

Os arquivos `_grid.scss` e `_grid-init.scss` devem ter a seguinte aparência:

![SCSS da Grade Responsiva do AEM](./assets/spa-bootstrap/aem-responsive-grid.png)

Agora, o SPA inclui o CSS necessário para oferecer suporte ao Modo de layout do AEM para componentes adicionados a um contêiner do AEM.

## Classes de utilitário

Copie as seguintes classes de utilitários no projeto de aplicativo React.

* [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
* [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
* [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
* [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) to `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![Classes de utilitário SPA remoto](./assets/spa-bootstrap/utility-classes.png)

## Iniciar o SPA

Agora que o SPA foi inicializado para integração com o AEM, vamos executar o SPA e ver como ele se parece!

1. Na linha de comando, navegue até a raiz do projeto de SPA
1. Inicie o SPA usando os comandos normais (se você ainda não tiver feito isso)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Navegue pelo SPA em [http://localhost:3000](http://localhost:3000). Tudo deve ficar bem!

![SPA em execução em http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Abra o SPA no AEM SPA Editor

Com o SPA em execução em [http://localhost:3000](http://localhost:3000), vamos abri-lo usando o AEM SPA Editor. Nada é editável no SPA ainda, isso só valida o SPA no AEM.

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > us > en__
1. Selecione a __Página Inicial do Aplicativo WKND__ e toque em __Editar__, e o SPA será exibido.

   ![Editar Página Inicial do Aplicativo WKND](./assets/spa-bootstrap/edit-home.png)

1. Alternar para __Visualização__ usando o alternador de modo no canto superior direito
1. Clique ao redor do SPA

   ![SPA em execução em http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Parabéns!

Você inicializou o SPA remoto para ser compatível com o AEM SPA Editor! Agora você sabe como:

* Adicionar as dependências npm do SDK JS do Editor SPA do AEM ao projeto SPA
* Configurar as variáveis de ambiente do SPA
* Integrar a API ModelManager ao SPA
* Configure um proxy interno para o SPA para que ele encaminhe as solicitações de conteúdo apropriadas para o AEM
* Resolver problemas com recursos estáticos de SPA que são resolvidos no contexto do Editor de SPA
* Adicionar CSS de grade responsiva do AEM para oferecer suporte à definição de layout em containers editáveis do AEM

## Próximas etapas

Agora que atingimos uma linha de base de compatibilidade com o AEM SPA Editor, podemos começar a introduzir áreas editáveis. Primeiro, observamos como colocar um [componente editável fixo](./spa-fixed-component.md) no SPA.
