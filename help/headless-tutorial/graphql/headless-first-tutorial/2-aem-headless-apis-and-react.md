---
title: APIs do AEM Headless e React - primeiro tutorial do AEM Headless
description: Saiba como cobrir a recuperação de dados do Fragmento de conteúdo das APIs do GraphQL do AEM e exibi-los no aplicativo React.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 225
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# APIs do AEM Headless e React

Bem-vindo a este capítulo de tutorial, onde exploraremos a configuração de um aplicativo React para se conectar com as APIs do Adobe Experience Manager (AEM) Headless usando o AEM Headless SDK. Abordaremos a recuperação de dados do Fragmento de conteúdo das APIs do GraphQL do AEM e sua exibição no aplicativo React.

As APIs do AEM Headless permitem acessar o conteúdo do AEM de qualquer aplicativo cliente. Vamos orientá-lo na configuração do aplicativo React para se conectar às APIs do AEM Headless usando o AEM Headless SDK. Essa configuração estabelece um canal de comunicação reutilizável entre o aplicativo React e o AEM.

Em seguida, usaremos o AEM Headless SDK para recuperar dados do fragmento de conteúdo das APIs do GraphQL da AEM. Os fragmentos de conteúdo no AEM fornecem gerenciamento de conteúdo estruturado. Ao usar o AEM Headless SDK, você pode consultar e buscar facilmente os dados do fragmento de conteúdo usando o GraphQL.

Depois que tivermos os dados do fragmento de conteúdo, nós os integraremos ao seu aplicativo React. Você aprenderá a formatar e exibir os dados de maneira atraente. Abordaremos as práticas recomendadas para manipular e renderizar dados do Fragmento de conteúdo em componentes React, garantindo uma integração perfeita com a interface do usuário do seu aplicativo.

Ao longo do tutorial, forneceremos explicações, exemplos de código e dicas práticas. Ao final, você poderá configurar seu aplicativo React para se conectar às APIs do AEM Headless, recuperar dados do fragmento de conteúdo usando o AEM Headless SDK e exibi-los facilmente no aplicativo React. Vamos começar!


## Clonar o aplicativo React

1. Clonar o aplicativo do [Github](https://github.com/lamontacrook/headless-first/tree/main) executando o seguinte comando na linha de comando.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Altere para o diretório `headless-first` e instale as dependências.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurar o aplicativo React

1. Crie um arquivo chamado `.env` na raiz do projeto. Em `.env`, defina os seguintes valores:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Você pode recuperar um token de desenvolvedor no Cloud Manager. Faça logon no [Adobe Cloud Manager](https://experience.adobe.com/). Clique em __Experience Manager > Cloud Manager__. Escolha o Programa apropriado e clique nas reticências ao lado do Ambiente.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. Clique na guia __Integrações__
   1. Clique na guia __Token local e no botão Obter token de desenvolvimento local__
   1. Copie o token de acesso começando após a cotação aberta e até antes da cotação fechada.
   1. Cole o token copiado como o valor de `REACT_APP_TOKEN` no arquivo `.env`.
   1. Agora vamos criar o aplicativo executando `npm ci` na linha de comando.
   1. Agora inicie o aplicativo React e execute `npm run start` na linha de comando.
   1. Em [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) um arquivo chamado `context.js` inclui o código para definir os valores no arquivo `.env` no contexto do aplicativo.

## Executar o aplicativo React

1. Inicie o aplicativo React executando `npm run start` na linha de comando.

   ```
   $ npm run start
   ```

   O aplicativo React iniciará e abrirá uma janela do navegador para `http://localhost:3000`. As alterações no aplicativo React serão recarregadas automaticamente no navegador.

## Conectar-se a APIs do AEM Headless

1. Para conectar o aplicativo React ao AEM as a Cloud Service, vamos adicionar algumas coisas a `App.js`. Na importação de `React`, adicione `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importar `AppContext` do arquivo `context.js`.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Agora, no código do aplicativo, defina uma variável de contexto.

   ```javascript
   const context = useContext(AppContext);
   ```

   E, por fim, envolva o código de retorno em `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Para referência, o `App.js` agora deve ser assim.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. Importe o SDK `AEMHeadless`. Esta SDK é uma biblioteca auxiliar usada pelo aplicativo para interagir com as APIs headless do AEM.

   Adicionar esta instrução de importação ao `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Adicione o seguinte `{ useContext, useEffect, useState }` à instrução de importação ` React`.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importe o `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dentro do componente `Home`, obtenha a variável `context` de `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inicialize o AEM Headless SDK dentro de um `useEffect()`, já que o AEM Headless SDK deve ser alterado quando a variável `context` for alterada.

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > Há um arquivo `context.js` em `/utils` que está lendo elementos do arquivo `.env`. Para referência, o `context.url` é a URL do ambiente do AEM as a Cloud Service. O `context.endpoint` é o caminho completo para o ponto de extremidade criado na lição anterior. Por fim, `context.token` é o token do desenvolvedor.


1. Crie o estado React que expõe o conteúdo proveniente do AEM Headless SDK.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Conecte o aplicativo ao AEM. Use a consulta persistente criada na lição anterior. Vamos adicionar o seguinte código dentro do `useEffect` depois que o AEM Headless SDK for inicializado. Torne `useEffect` dependente da variável `context`, como visto abaixo.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. Abra a exibição de rede das ferramentas do desenvolvedor para revisar a solicitação do GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Ferramentas de Desenvolvimento do Chrome](./assets/2/dev-tools.png)

   O AEM Headless SDK codifica a solicitação do GraphQL e adiciona os parâmetros fornecidos. Você pode abrir a solicitação no navegador.

   >[!NOTE]
   >
   > Como a solicitação vai para o ambiente de autor, é necessário fazer logon no ambiente em outra guia do mesmo navegador.


## Renderizar conteúdo do fragmento de conteúdo

1. Exiba os fragmentos de conteúdo no aplicativo. Retorne um `<div>` com o título do teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Você deve ver o campo de título do teaser exibido na tela.

1. A última etapa é adicionar o teaser à página. Um componente de teaser do React é incluído no pacote. Primeiro, vamos incluir a importação. Na parte superior do arquivo `home.js`, adicione a linha:

   `import Teaser from '../../components/teaser/teaser';`

   Atualize a declaração de devolução:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Agora você deve ver o teaser com o conteúdo incluído no fragmento.


## Próximas etapas

Parabéns! Você atualizou com sucesso o aplicativo React para integrar-se às APIs do AEM Headless usando o SDK do AEM Headless.

Em seguida, vamos criar um componente mais complexo da Lista de imagens que renderiza dinamicamente os fragmentos de conteúdo referenciados do AEM.

[Próximo capítulo: Criar um componente de lista de imagens](./3-complex-components.md)
