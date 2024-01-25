---
title: APIs sem periféricos de AEM e React - Primeiro tutorial sobre AEM sem periféricos
description: Saiba como cobrir a recuperação de dados do Fragmento de conteúdo de APIs do AEM GraphQL e exibi-los no aplicativo React.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 280
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# APIs AEM Headless e React

Bem-vindo a este capítulo de tutorial, onde exploraremos a configuração de um aplicativo React para se conectar com APIs headless do Adobe Experience Manager (AEM) usando o SDK headless do AEM. Abordaremos a recuperação de dados do Fragmento de conteúdo de APIs do AEM GraphQL e sua exibição no aplicativo React.

As APIs do AEM Headless permitem acessar conteúdo do AEM de qualquer aplicativo cliente. Vamos orientá-lo na configuração do aplicativo React para se conectar a APIs AEM headless usando o SDK AEM headless. Essa configuração estabelece um canal de comunicação reutilizável entre seu aplicativo React e o AEM.

Em seguida, usaremos o SDK AEM Headless para recuperar dados do Fragmento de conteúdo de APIs do AEM GraphQL. Fragmentos de conteúdo no AEM fornecem gerenciamento de conteúdo estruturado. Ao usar o SDK do AEM headless, é possível consultar e buscar facilmente dados do fragmento de conteúdo usando o GraphQL.

Depois que tivermos os dados do fragmento de conteúdo, nós os integraremos ao seu aplicativo React. Você aprenderá a formatar e exibir os dados de maneira atraente. Abordaremos as práticas recomendadas para manipular e renderizar dados do Fragmento de conteúdo em componentes React, garantindo uma integração perfeita com a interface do usuário do seu aplicativo.

Ao longo do tutorial, forneceremos explicações, exemplos de código e dicas práticas. Ao final, você poderá configurar seu aplicativo React para se conectar a APIs AEM Headless, recuperar dados do fragmento de conteúdo usando o SDK AEM Headless e exibi-los facilmente no aplicativo React. Vamos começar!


## Clonar o aplicativo React

1. Clonar o aplicativo de [Github](https://github.com/lamontacrook/headless-first/tree/main) executando o seguinte comando na linha de comando.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. Altere para `headless-first` e instale as dependências.

   ```
   $ cd headless-first
   $ npm ci
   ```

## Configurar o aplicativo React

1. Crie um arquivo chamado `.env` na raiz do projeto. Entrada `.env` defina os seguintes valores:

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Você pode recuperar um token de desenvolvedor no Cloud Manager. Efetue logon no [Adobe Cloud Manager](https://experience.adobe.com/). Clique em __Experience Manager > Cloud Manager__. Escolha o Programa apropriado e clique nas reticências ao lado do Ambiente.

   ![Console do desenvolvedor do AEM](./assets/2/developer-console.png)

   1. Clique em __Integrações__ guia
   1. Clique em __Guia Token local e obter token de desenvolvimento local__ botão
   1. Copie o token de acesso começando após a cotação aberta e até antes da cotação fechada.
   1. Cole o token copiado como o valor de `REACT_APP_TOKEN` no `.env` arquivo.
   1. Agora vamos criar o aplicativo executando `npm ci` na linha de comando.
   1. Agora inicie o aplicativo React e execute `npm run start` na linha de comando.
   1. Entrada [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) um arquivo chamado `context.js`  inclui o código para definir os valores na variável `.env` no contexto do aplicativo.

## Executar o aplicativo React

1. Inicie o aplicativo React executando `npm run start` na linha de comando.

   ```
   $ npm run start
   ```

   O aplicativo React será iniciado e abrirá uma janela do navegador para `http://localhost:3000`. As alterações no aplicativo React serão recarregadas automaticamente no navegador.

## Conexão com APIs AEM Headless

1. Para conectar o aplicativo React ao AEM as a Cloud Service, vamos adicionar algumas coisas a `App.js`. No `React` importar, adicionar `useContext`.

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

   E, por fim, inclua o código de retorno em `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   A título de referência, `App.js` Agora deve ser assim.

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

1. Importe o `AEMHeadless` SDK. Este SDK é uma biblioteca auxiliar usada pelo aplicativo para interagir com APIs AEM Headless.

   Adicione esta declaração de importação à `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   Adicione o seguinte `{ useContext, useEffect, useState }` para o` React` declaração de importação.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   Importe o `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   Dentro do `Home` componente, obtenha o `context` variável do `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. Inicialize o SDK do AEM Headless em um  `useEffect()`, já que o SDK AEM Headless deve ser alterado quando a variável  `context` alterações na variável.

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
   > Existe uma `context.js` arquivo em `/utils` que está lendo elementos do `.env` arquivo. A título de referência, `context.url` é o URL do ambiente as a Cloud Service AEM. A variável `context.endpoint` é o caminho completo para o endpoint criado na lição anterior. Por último, a `context.token` é o token do desenvolvedor.


1. Crie o estado React que expõe o conteúdo proveniente do SDK AEM Headless.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. Conecte o aplicativo ao AEM. Use a consulta persistente criada na lição anterior. Vamos adicionar o seguinte código à `useEffect` após o SDK AEM headless ser inicializado. Faça com que o `useEffect` dependente do  `context` como visto abaixo.


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

   ![Ferramentas de desenvolvimento do Chrome](./assets/2/dev-tools.png)

   O SDK do AEM Headless codifica a solicitação do GraphQL e adiciona os parâmetros fornecidos. Você pode abrir a solicitação no navegador.

   >[!NOTE]
   >
   > Como a solicitação vai para o ambiente de autor, é necessário fazer logon no ambiente em outra guia do mesmo navegador.


## Renderizar conteúdo do fragmento de conteúdo

1. Exiba os fragmentos de conteúdo no aplicativo. Retornar um `<div>` com o título do teaser.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   Você deve ver o campo de título do teaser exibido na tela.

1. A última etapa é adicionar o teaser à página. Um componente de teaser do React é incluído no pacote. Primeiro, vamos incluir a importação. Na parte superior do `home.js` adicione a linha:

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

Parabéns! Você atualizou com sucesso o aplicativo React para integrar com APIs AEM Headless usando o SDK AEM Headless!

Em seguida, vamos criar um componente de Lista de imagens mais complexo que renderiza dinamicamente fragmentos de conteúdo referenciados do AEM.

[Próximo capítulo: Criar um componente de lista de imagens](./3-complex-components.md)
