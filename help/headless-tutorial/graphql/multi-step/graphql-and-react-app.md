---
title: Criar um aplicativo React que Consultas AEM usando a API GraphQL - Introdução ao AEM Headless - GraphQL
description: Introdução à Adobe Experience Manager (AEM) e GraphQL. Crie um aplicativo React que busca conteúdo/dados AEM API GraphQL e veja também como AEM SDK JS headless é usado.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 2%

---


# Criar um aplicativo React que use APIs GraphQL AEM

Neste capítulo, você explora como AEM APIs GraphQL podem impulsionar a experiência em um aplicativo externo.

Um aplicativo React simples é usado para consultar e exibir **Equipe** e **Pessoa** conteúdo exposto por APIs GraphQL AEM. O uso do React não é, em grande medida, importante e a aplicação externa de consumo poderia ser escrita em qualquer estrutura para qualquer plataforma.

## Pré-requisitos

Pressupõe-se que as etapas descritas nas partes anteriores deste tutorial multiparte foram concluídas, ou [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) O é instalado nos serviços de criação e publicação do AEM as a Cloud Service.

_As capturas de tela do IDE neste capítulo vêm de [Código do Visual Studio](https://code.visualstudio.com/)_

O seguinte software deve ser instalado:

- [Node.js v14+](https://nodejs.org/en/)
- [npm 6.4+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Código do Visual Studio](https://code.visualstudio.com/)

## Objetivos

Saiba como:

- Baixe e inicie o exemplo React app
- Consulte AEM pontos finais GraphQL usando o [SDK JS sem cabeçalho AEM](https://github.com/adobe/aem-headless-client-js)
- AEM de consulta para uma lista de equipes e seus membros referenciados
- AEM de consulta para detalhes de um membro da equipe

## Obter a amostra do aplicativo React

Neste capítulo, um aplicativo React de amostra detalhado é implementado com o código necessário para interagir com AEM API GraphQL e exibe os dados de equipe e pessoa obtidos deles.

A amostra do código-fonte do aplicativo React está disponível em Github.com em <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Para obter o aplicativo React:

1. Clonar o aplicativo WKND GraphQL de amostra do [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navegar para `basic-tutorial` e abra-a no IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![Reagir aplicativo no VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Atualizar `.env.development` para se conectar AEM serviço de publicação as a Cloud Service.

   - Definir `REACT_APP_HOST_URI`O valor de para ser o URL de publicação do AEM as a Cloud Service (por exemplo, `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) e `REACT_APP_AUTH_METHOD`Valor de &#39;s&#39; para `none`
   >[!NOTE]
   >
   > Certifique-se de ter publicado a configuração do projeto, os modelos de Fragmento de conteúdo, os Fragmentos de conteúdo criados, os pontos de extremidade GraphQL e as consultas persistentes das etapas anteriores.
   >
   > Se você executou as etapas acima no SDK de autor do AEM local, é possível apontar para `http://localhost:4502` e `REACT_APP_AUTH_METHOD`Valor de &#39;s&#39; para `basic`.


1. Na linha de comando, vá para a `aem-guides-wknd-graphql/basic-tutorial` pasta

1. Inicie o aplicativo React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. O aplicativo React é iniciado no modo de desenvolvimento em [http://localhost:3000/](http://localhost:3000/). As alterações feitas no aplicativo React no tutorial são refletidas imediatamente.

![Aplicativo React parcialmente implementado](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Trata-se de um React App parcialmente implementado, seguindo as etapas até sua conclusão. Os arquivos JavaScript que precisam de implementação têm o seguinte comentário, certifique-se de adicionar/atualizar o código nesses arquivos com o código especificado neste tutorial.
>
>
> //****************************************
>
>  // TODO : Implemente isso seguindo as etapas AEM tutorial autônomo
>
>  //****************************************



## Anatomia do aplicativo React

O aplicativo React de amostra tem três partes principais:

1. O `src/api` contém arquivos usados para fazer consultas GraphQL para AEM.
   - `src/api/aemHeadlessClient.js` inicializa e exporta o Cliente Sem Cabeçalho AEM usado para se comunicar com AEM
   - `src/api/usePersistedQueries.js` implementa [ganchos de reação personalizados](https://reactjs.org/docs/hooks-custom.html) retornar dados de AEM GraphQL para o `Teams.js` e `Person.js` exibir componentes.

1. O `src/components/Teams.js` O arquivo exibe uma lista de equipes e seus membros, usando uma consulta de lista.
1. O `src/components/Person.js` arquivo exibe os detalhes de uma única pessoa, usando uma consulta parametrizada de resultado único.

## Revise o objeto AEMHeadless

Revise o `aemHeadlessClient.js` para saber como criar o `AEMHeadless` objeto usado para comunicação com o AEM.

1. Abrir `src/api/aemHeadlessClient.js`.

1. Revise as linhas 1 a 40:

   - A importação `AEMHeadless` declaração da [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js), linha 11.

   - A configuração de autorização com base em variáveis definidas no `.env.development`, linha 14-22 e, a expressão da função de seta `setAuthorization`, linha 31-40.

   - O `serviceUrl` configuração para o [proxy de desenvolvimento](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configuração, linha 27.

1. As linhas 42-49 são mais importantes, pois instanciam o `AEMHeadless` cliente e exporte-o para uso no aplicativo React.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Implementar para executar AEM consultas persistentes de GraphQL

Abra o `usePersistedQueries.js` arquivo para implementar o genérico `fetchPersistedQuery(..)` para executar as consultas persistentes de GraphQL AEM. O `fetchPersistedQuery(..)` usa a função `aemHeadlessClient` do objeto `runPersistedQuery()` para executar o query de forma assíncrona, com comportamento baseado em promessas.

Mais tarde, reação personalizada `useEffect` hook chama essa função para recuperar dados específicos do AEM.

1. Em `src/api/usePersistedQueries.js` **atualizar** `fetchPersistedQuery(..)`, linha 35, com o código abaixo.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## Implementar a funcionalidade de equipes

Em seguida, crie a funcionalidade para exibir as equipes e seus membros na exibição principal do aplicativo React. Essa funcionalidade exige:

- Um novo [gancho useEffect do React personalizado](https://reactjs.org/docs/hooks-custom.html) em `src/api/usePersistedQueries.js` que chama o `my-project/all-teams` consulta persistente, retornando uma lista de Fragmentos de conteúdo de equipe no AEM.
- Um componente React em `src/components/Teams.js` que chama o novo Reato personalizado `useEffect` gancho e renderiza os dados das equipes.

Uma vez concluída, a visualização principal do aplicativo é preenchida com os dados das equipes do AEM.

![Exibição de equipes](./assets/graphql-and-external-app/react-app__teams-view.png)

### Etapas

1. Abrir `src/api/usePersistedQueries.js`.

1. Localize a função `useAllTeams()`

1. Para criar um `useEffect` gancho que chama a consulta persistente `my-project/all-teams` via `fetchPersistedQuery(..)`, adicione o seguinte código. O gancho também retorna somente os dados relevantes da resposta GraphQL da AEM em `data?.teamList?.items`, permitindo que os componentes de visualização React sejam agnósticos das estruturas JSON pai.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. Abrir `src/components/Teams.js`

1. No `Teams` Reagir componente, busque a lista de equipes do AEM usando o `useAllTeams()` gancho.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Execute a validação de dados com base em exibição, exibindo uma mensagem de erro ou um indicador de carregamento com base nos dados retornados.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. Por fim, renderize os dados das equipes. Cada equipe retornada da consulta GraphQL é renderizada usando o `Team` Reagir subcomponente.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## Implementar a funcionalidade Pessoa

Com o [Funcionalidade de equipes](#implement-teams-functionality) Concluído, vamos implementar a funcionalidade para lidar com a exibição nos detalhes de um membro da equipe ou de uma pessoa.

Essa funcionalidade exige:

- Um novo [gancho useEffect do React personalizado](https://reactjs.org/docs/hooks-custom.html) em `src/api/usePersistedQueries.js` que chama o parametrizado `my-project/person-by-name` consulta persistente e retorna um registro de pessoa única.

- Um componente React em `src/components/Person.js` que usa o nome completo de uma pessoa como parâmetro de consulta, chama o novo React personalizado `useEffect` gancho e renderiza os dados da pessoa.

Uma vez concluído, selecionar o nome de uma pessoa na exibição Equipes renderiza a exibição da pessoa.

![Person](./assets/graphql-and-external-app/react-app__person-view.png)

1. Abrir `src/api/usePersistedQueries.js`.

1. Localize a função `usePersonByName(fullName)`

1. Para criar um `useEffect` gancho que chama a consulta persistente `my-project/all-teams` via `fetchPersistedQuery(..)`, adicione o seguinte código. O gancho também retorna somente os dados relevantes da resposta GraphQL da AEM em `data?.teamList?.items`, permitindo que os componentes de visualização React sejam agnósticos das estruturas JSON pai.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. Abrir `src/components/Person.js`
1. No `Person` Componente de reação, analise o `fullName` parâmetro de rota e buscar os dados de pessoa do AEM usando o `usePersonByName(fullName)` gancho.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. Execute a validação de dados com base na visualização, exibindo uma mensagem de erro ou um indicador de carregamento com base nos dados retornados.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. Por fim, renderize os dados da pessoa.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## Experimente o aplicativo

Revisar o aplicativo [http://localhost:3000/](http://localhost:3000/) e clique em _Membros_ links. Além disso, é possível adicionar mais equipes e/ou membros ao Alfa da equipe, adicionando Fragmentos de conteúdo em AEM.

## Sob O Capuz

Abra o **Ferramentas do desenvolvedor** > **Rede** e _Filtro_ para `all-teams` , você percebe a solicitação da API GraphQL `/graphql/execute.json/my-project/all-teams` é contra `http://localhost:3000` e **NOT** contra o valor de `REACT_APP_HOST_URI`(por exemplo, <https://publish-p123-e456.adobeaemcloud.com>), isso ocorre porque [configuração de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) usar `http-proxy-middleware` módulo.


![Solicitação de API GraphQL via Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Revise o `../setupProxy.js` e no `../proxy/setupProxy.auth.**.js` os arquivos observam como `/content` e `/graphql` os caminhos são proxy e indicados: não é um ativo estático.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

No entanto, essa não é uma opção adequada para implantação de produção, e mais detalhes podem ser encontrados em _Implantação de produção_ seção.

## Parabéns. {#congratulations}

Parabéns. Você criou com sucesso o aplicativo React para consumir e exibir dados das APIs GraphQL AEM como parte do tutorial básico!
