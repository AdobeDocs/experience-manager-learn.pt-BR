---
title: Bootstrap do SPA remoto para o editor SPA
description: Saiba como inicializar um SPA AEM remoto para compatibilidade com o editor SPA.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---

# Bootstrap do SPA remoto para o editor SPA

Antes de ser possível adicionar as áreas editáveis ao SPA AEM remoto, ele deve ser inicializado com o SDK JavaScript do editor do SPA e algumas outras configurações.

## AEM Instalar dependências npm do editor JS do SPA

AEM Primeiro, analise as dependências de SPA npm para o projeto React, e instale-as.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : fornece a API para recuperar conteúdo do AEM.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : fornece a API que mapeia o conteúdo AEM para componentes SPA.
+ [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : fornece uma API para a compilação de componentes personalizados de SPA e fornece implementações de uso comum, como o componente React `AEMPage`.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## Revisar variáveis de ambiente do SPA

Várias variáveis de ambiente devem ser expostas ao SPA remoto para que ele saiba como interagir com AEM.

1. Abrir projeto SPA remoto em `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` no IDE
1. Abrir o arquivo `.env.development`
1. No arquivo, preste atenção específica às chaves e atualize conforme necessário:

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![Variáveis de ambiente do SPA Remoto](./assets/spa-bootstrap/env-variables.png)

   *Lembre-se de que as variáveis de ambiente personalizadas no React devem receber o prefixo `REACT_APP_`.*

   + `REACT_APP_HOST_URI`: o esquema e o host do serviço AEM ao qual o SPA Remoto se conecta.
      + Esse valor muda com base no tipo de serviço AEM (local, Desenvolvimento, Preparo ou Produção) e AEM (Autor vs. Publish)
   + `REACT_APP_USE_PROXY`: isso evita problemas do CORS durante o desenvolvimento, informando o servidor de desenvolvimento do react às solicitações do AEM do proxy, como `/content, /graphql, .model.json` usando o módulo `http-proxy-middleware`.
   + `REACT_APP_AUTH_METHOD`: método de autenticação para solicitações enviadas por AEM, as opções são &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; ou deixe em branco para caso de uso sem autenticação
      + Obrigatório para uso com o AEM Author
      + Possivelmente necessário para uso com AEM Publish (se o conteúdo estiver protegido)
      + O desenvolvimento em relação ao SDK do AEM é compatível com contas locais via Autenticação básica. Este é o método usado neste tutorial.
      + Ao integrar com o AEM as a Cloud Service, use [tokens de acesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=pt-BR)
   + `REACT_APP_BASIC_AUTH_USER`: o AEM __username__ do SPA para autenticar ao recuperar o conteúdo do AEM.
   + `REACT_APP_BASIC_AUTH_PASS`: a __senha__ do AEM SPA para autenticar ao recuperar o conteúdo do AEM.

## Integrar a API ModelManager

AEM Com as dependências SPA npm disponíveis para o aplicativo, inicializar AEM `ModelManager` no `index.js` do projeto antes de `ReactDOM.render(...)` ser chamado.

O [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) é responsável pela conexão com o AEM para recuperar o conteúdo editável.

1. Abra o projeto do SPA remoto no IDE
1. Abrir o arquivo `src/index.js`
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

## Configurar um proxy SPA interno

Ao criar um SPA editável, é melhor configurar um [proxy interno no SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), que seja configurado para rotear as solicitações apropriadas para o AEM. Isso é feito usando o módulo [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm, que já está instalado pelo aplicativo WKND GraphQL base.

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

   1. Solicitações específicas de proxy feitas ao SPA (`http://localhost:3000`) para AEM `http://localhost:4502`
      + Somente solicitações de proxy cujos caminhos correspondem a padrões que indicam que devem ser atendidos pelo AEM, conforme definido em `toAEM(path, req)`.
      + Ele reescreve caminhos de SPA para suas páginas de AEM correspondentes, conforme definido em `pathRewriteToAEM(path, req)`
   1. Ele adiciona cabeçalhos CORS a todas as solicitações para permitir acesso a conteúdo AEM, conforme definido por `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + Se isso não for adicionado, erros do CORS ocorrem ao carregar o conteúdo de AEM no SPA.

1. Abrir o arquivo `src/setupProxy.js`
1. Revise a linha que aponta para o arquivo de configuração de proxy `setupProxy.spa-editor.auth.basic`:

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

Observe que qualquer alteração no `src/setupProxy.js` ou em seus arquivos referenciados requer a reinicialização do SPA.

## Recurso estático de SPA

Os recursos estáticos de SPA, como o Logotipo WKND e o Carregamento de gráficos, precisam ter seus URLs src atualizados para forçá-los a serem carregados do host remoto SPA. Se relativo, quando o SPA é carregado no Editor SPA para criação, esses URLs assumem como padrão o uso do host do AEM SPA em vez do, resultando em 404 solicitações, conforme ilustrado na imagem abaixo.

![Recursos estáticos desfeitos](./assets/spa-bootstrap/broken-static-resource.png)

Para resolver esse problema, faça com que um recurso estático hospedado pelo SPA remoto use caminhos absolutos que incluem a origem SPA remoto.

1. Abra o projeto SPA no IDE
1. Abra o arquivo de variáveis de ambiente SPA `src/.env.development` e adicione uma variável para o URI público SPA:

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Ao implantar no AEM as a Cloud Service, você precisa fazer o mesmo para os `.env` arquivos correspondentes._

1. Abrir o arquivo `src/App.js`
1. Importar o URI público do SPA das variáveis de ambiente SPA

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

## Grade responsiva AEM

Para dar suporte ao modo de layout do Editor de SPA para áreas editáveis no SPA, devemos integrar o CSS de Grade Responsiva do AEM ao SPA. Não se preocupe - esse sistema de grade é aplicável apenas aos contêineres editáveis e você pode usar o sistema de grade de sua escolha para direcionar o layout do restante do SPA.

Adicione os arquivos AEM Responsive Grid SCSS ao SPA.

1. Abra o projeto SPA no IDE
1. Baixe e copie os dois arquivos a seguir em `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + O gerador de SCSS da grade responsiva AEM
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Invoca `_grid.scss` usando os pontos de interrupção específicos do SPA (desktop e dispositivos móveis) e colunas (12).
1. Abrir `src/App.scss` e importar `./styles/grid-init.scss`

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

Os arquivos `_grid.scss` e `_grid-init.scss` devem ter a seguinte aparência:

![SCSS da Grade Responsiva do AEM](./assets/spa-bootstrap/aem-responsive-grid.png)

Agora, o SPA inclui o CSS necessário para oferecer suporte ao Modo de layout de AEM para componentes adicionados a um contêiner AEM.

## Classes de utilitário

Copie as seguintes classes de utilitários no projeto de aplicativo React.

+ [RoutedLink.js](./assets/spa-bootstrap/RoutedLink.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
+ [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`
+ [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js) a `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`
+ [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js) to `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`

![Classes de utilitário SPA remoto](./assets/spa-bootstrap/utility-classes.png)

## Iniciar o SPA

Agora que o SPA está bootstrapped para integração com AEM, vamos executar o SPA e ver como ele se parece!

1. Na linha de comando, navegue até a raiz do projeto SPA
1. Inicie o SPA usando os comandos normais (se você ainda não tiver feito isso)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. Procurar o SPA em [http://localhost:3000](http://localhost:3000). Tudo deve ficar bem!

![SPA em execução em http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Abrir o SPA no Editor SPA do AEM

Com o SPA em execução em [http://localhost:3000](http://localhost:3000), vamos abri-lo usando o Editor SPA do AEM. Nada é editável no SPA ainda, isso só valida o SPA no AEM.

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > us > en__
1. Selecione a __Página Inicial do Aplicativo WKND__ e toque em __Editar__ e o SPA será exibido.

   ![Editar Página Inicial do Aplicativo WKND](./assets/spa-bootstrap/edit-home.png)

1. Alternar para __Visualização__ usando o alternador de modo no canto superior direito
1. Clique ao redor do SPA

   ![SPA em execução em http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Parabéns.

Você inicializou o SPA remoto para ser compatível com o Editor de SPA do AEM! Agora você sabe como:

+ Adicionar as dependências npm do SDK JS do SPA AEM Editor do ao projeto SPA
+ Configurar as variáveis de ambiente do SPA
+ Integrar a API ModelManager ao SPA
+ Configurar um proxy interno do SPA para que ele encaminhe as solicitações de conteúdo apropriadas para o AEM
+ Resolver problemas com recursos estáticos do SPA resolvidos no contexto do Editor de SPA
+ Adicionar CSS de grade responsiva do AEM para oferecer suporte à definição de layout em containers editáveis do AEM

## Próximas etapas

AEM Agora que atingimos uma linha de base de compatibilidade com o SPA Editor, podemos começar a introduzir áreas editáveis. Primeiro, analisamos como colocar um [componente editável fixo](./spa-fixed-component.md) no SPA.
