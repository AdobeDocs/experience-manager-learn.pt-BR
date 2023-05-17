---
title: AEM APIs headless e React - Primeiro tutorial AEM headless
description: Saiba como cobrir a recuperação de dados do Fragmento de conteúdo AEM APIs do GraphQL e sua exibição no aplicativo React.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# AEM APIs headless e React

Bem-vindo a este capítulo tutorial, onde exploraremos a configuração de um aplicativo React para conexão com APIs headless do Adobe Experience Manager (AEM) usando o SDK headless do AEM. Abordaremos a recuperação de dados do Fragmento de conteúdo AEM APIs do GraphQL e a exibição deles no aplicativo React.

AEM APIs headless permitem acessar AEM conteúdo de qualquer aplicativo cliente. Vamos orientá-lo na configuração do aplicativo React para se conectar a AEM APIs sem cabeçalho usando o SDK sem cabeçalho AEM. Essa configuração estabelece um canal de comunicação reutilizável entre o aplicativo React e o AEM.

Em seguida, usaremos o SDK sem cabeçalho AEM para recuperar os dados do Fragmento de conteúdo AEM APIs do GraphQL. Os Fragmentos de conteúdo no AEM fornecem gerenciamento de conteúdo estruturado. Ao utilizar o SDK sem cabeçalho AEM, é possível consultar e buscar facilmente os dados do Fragmento de conteúdo usando o GraphQL.

Depois de ter os dados do Fragmento de conteúdo, nós os integraremos ao seu aplicativo React. Você aprenderá a formatar e exibir os dados de maneira atraente. Abordaremos as práticas recomendadas para lidar e renderizar os dados do Fragmento de conteúdo nos componentes do React, garantindo uma integração perfeita com a interface do usuário do seu aplicativo.

Durante todo o tutorial, forneceremos explicações, exemplos de código e dicas práticas. No final, você poderá configurar o aplicativo React para se conectar AEM APIs sem cabeçalho, recuperar dados do Fragmento de conteúdo usando o SDK sem cabeçalho AEM e exibi-los facilmente no aplicativo React. Vamos começar!


## Clonar o aplicativo React

1. Clonar o aplicativo de [Github](https://github.com/lamontacrook/headless-first/tree/main) executando o seguinte comando na linha de comando.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Alterar para o `headless-first` e instale as dependências.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurar o aplicativo React

1. Crie um arquivo com o nome `.env` na raiz do projeto. Em `.env` defina os seguintes valores:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Você pode recuperar um token de desenvolvedor no Cloud Manager. Faça logon em [Adobe Cloud Manager](https://experience.adobe.com/). Clique em __Experience Manager > Cloud Manager__. Escolha o Programa apropriado e clique nas reticências ao lado de Ambiente.

   ![Console do desenvolvedor do AEM](./assets/2/developer-console.png)

   1. Clique no botão __Integrações__ guia
   1. Clique em __Guia Token local e Obter token de desenvolvimento local__ botão
   1. Copie o token de acesso que começa após a aspa aberta até antes da aspa fechada.
   1. Cole o token copiado como o valor de `REACT_APP_TOKEN` no `.env` arquivo.
   1. Agora vamos criar o aplicativo executando `npm ci` na linha de comando.
   1. Agora inicie o aplicativo React e executando `npm run start` na linha de comando.
   1. Em [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) um arquivo chamado `context.js`  O inclui o código para definir os valores no `.env` no contexto do aplicativo.

## Executar o aplicativo React

1. Inicie o aplicativo React executando `npm run start` na linha de comando.

   ```
   $ npm run start
   ```

   O React app iniciará e abrirá uma janela do navegador para `http://localhost:3000`. As alterações no aplicativo React serão automaticamente recarregadas no navegador.

## Conecte-se a AEM APIs headless

1. Para conectar o aplicativo React ao AEM as a Cloud Service, vamos adicionar algumas coisas ao `App.js`. No `React` importar, adicionar `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   Importar `AppContext` do `context.js` arquivo.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   Agora, no código do aplicativo, defina uma variável de contexto.

   ```javascript
   const context = useContext(AppContext);
   ```

   E, finalmente, envolva o código de retorno em `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   Para referência, a variável `App.js` deveria ser assim agora.

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

1. Importe o `AEMHeadless` SDK. Este SDK é uma biblioteca de ajuda usada pelo aplicativo para interagir com AEM APIs headless.

   Adicione esta declaração de importação ao `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Adicione o seguinte `{ useContext, useEffect, useState }` para` React` declaração de importação.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importe o `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dentro do `Home` componente, obtenha o `context` da variável `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inicialize o SDK sem cabeçalho AEM dentro de um  `useEffect()`, já que o SDK sem cabeçalho do AEM deve ser alterado quando a variável  `context` alterações na variável .

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
   > Existe um `context.js` arquivo em `/utils` que é a leitura de elementos do `.env` arquivo. Para referência, a variável `context.url` é o URL do ambiente as a Cloud Service AEM. O `context.endpoint` é o caminho completo para o endpoint criado na lição anterior. Por último, a `context.token` é o token do desenvolvedor.


1. Crie o estado React que expõe o conteúdo proveniente do SDK sem cabeçalho AEM.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Conecte o aplicativo ao AEM. Use a consulta persistente criada na lição anterior. Vamos adicionar o seguinte código dentro do `useEffect` após a inicialização do SDK sem cabeçalho do AEM. Faça o `useEffect` dependente do  `context` conforme mostrado abaixo.


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

1. Abra a exibição Rede das ferramentas do desenvolvedor para revisar a solicitação do GraphQL.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Ferramentas de desenvolvimento do Chrome](./assets/2/dev-tools.png)

   O SDK sem cabeçalho do AEM codifica a solicitação do GraphQL e adiciona os parâmetros fornecidos. Você pode abrir a solicitação no navegador.

   >[!NOTE]
   >
   > Como a solicitação vai para o ambiente do autor, você deve estar conectado ao ambiente em outra guia do mesmo navegador.


## Renderizar conteúdo do fragmento de conteúdo

1. Exibir os Fragmentos do conteúdo no aplicativo. Retornar um `<div>` com o título do teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Você deve ver o campo de título do teaser exibido na tela.

1. A última etapa é adicionar o teaser à página. Um componente de teaser React está incluído no pacote. Primeiro, vamos incluir a importação. Na parte superior do `home.js` , adicione a linha :

   `import Teaser from '../../components/teaser/teaser';`

   Atualize a declaração de retorno:

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   Agora, você deve ver o teaser com o conteúdo incluído no fragmento.


## Próximas etapas

Parabéns! Você atualizou com êxito o aplicativo React para integrar com AEM APIs headless usando o SDK sem cabeçalho AEM!

Em seguida, vamos criar um componente de Lista de imagens mais complexo que renderiza dinamicamente os Fragmentos de conteúdo referenciados do AEM.

[Próximo capítulo: Criar um componente de Lista de imagens](./3-complex-components.md)