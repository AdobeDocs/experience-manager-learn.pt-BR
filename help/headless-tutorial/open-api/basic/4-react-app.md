---
title: Criar aplicativo React com AEM OpenAPI | Tutorial headless parte 4
description: Introdução ao Adobe Experience Manager (AEM) e OpenAPI. Crie um aplicativo React que busca conteúdo/dados das APIs de entrega de fragmentos de conteúdo baseadas em OpenAPI do AEM.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 1%

---


# Criar um aplicativo React com as OpenAPIs de entrega de fragmentos de conteúdo do AEM

Neste capítulo, você explora como a entrega de fragmentos de conteúdo do AEM com APIs OpenAPI pode impulsionar a experiência em aplicativos externos.

Um aplicativo React simples é usado para solicitar e exibir conteúdo de **Equipe** e **Pessoa** exposto pela Entrega de fragmento de conteúdo do AEM com APIs OpenAPI. O uso do React não é muito importante e o aplicativo externo de consumo pode ser escrito em qualquer estrutura para qualquer plataforma, desde que possa fazer solicitações HTTP para o AEM as a Cloud Service.

## Pré-requisitos

Pressupõe-se que as etapas descritas nas partes anteriores deste tutorial de várias partes foram concluídas.

Os softwares a seguir devem ser instalados:

* [Node.js v22+](https://nodejs.org/en)
* [Código do Visual Studio](https://code.visualstudio.com/)

## Objetivos

Saiba como:

* Baixe e inicie o aplicativo React de exemplo.
* Chame a entrega de fragmento de conteúdo do AEM com APIs OpenAPI para obter uma lista de equipes e seus membros referenciados.
* Chame a entrega de fragmento de conteúdo do AEM com APIs OpenAPI para recuperar os detalhes de um membro da equipe.

## Configurar CORS no AEM as a Cloud Service

Este exemplo de aplicativo React é executado localmente (em `http://localhost:3000`) e se conecta à Entrega de fragmento de conteúdo do AEM do serviço de publicação do AEM com APIs OpenAPI. Para permitir essa conexão, o CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos entre origens) deve ser configurado no serviço de publicação (ou pré-visualização) do AEM.

Siga as [instruções sobre como configurar um SPA em execução em `http://localhost:3000` para permitir solicitações do CORS para o serviço de Publicação do AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains).

### Proxy CORS local

Como alternativa, para desenvolvimento, execute um [Proxy CORS local](https://www.npmjs.com/package/local-cors-proxy) que permita uma conexão compatível com CORS com o AEM.

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

Atualize o valor `--proxyUrl` para sua URL de Publicação (ou Visualização) do AEM.

Com o Proxy CORS local em execução, acesse as APIs de entrega de fragmento de conteúdo do AEM em `http://localhost:8010/proxy` para evitar problemas com CORS.

## Clonar a amostra do aplicativo React

Um aplicativo React de amostra reduzido é implementado com o código necessário para interagir com a entrega de fragmentos de conteúdo do AEM com APIs OpenAPI e exibir os dados de equipe e pessoa obtidos deles.

O exemplo de código-fonte do aplicativo React está [disponível em Github.com](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic).

Para obter o aplicativo React:

1. Clonar o aplicativo WKND OpenAPI React de exemplo de [Github.com](https://github.com/adobe/aem-tutorials) da [`headless_open-api_basic` marca](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. Navegue até a pasta `headless/open-api/basic` e abra-a no IDE.

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. Atualize o `.env` para conectar-se ao serviço de Publicação do AEM as a Cloud Service, pois é aqui que os Fragmentos de conteúdo são publicados. Isso pode apontar para o serviço de Visualização do AEM se você quiser testar o aplicativo com o serviço de Visualização do AEM (e os Fragmentos de conteúdo forem publicados lá).

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   Ao usar o [Proxy CORS Local](#local-cors-proxy), defina `REACT_APP_HOST_URI` como `http://localhost:8010/proxy`.

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. Iniciar o aplicativo React

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. O aplicativo React é iniciado no modo de desenvolvimento em [http://localhost:3000/](http://localhost:3000/). As alterações feitas no aplicativo React em todo o tutorial são refletidas imediatamente no navegador da Web.

>[!IMPORTANT]
>
>   Este aplicativo React está parcialmente implementado. Siga as etapas deste tutorial para concluir a implementação. Os arquivos do JavaScript que precisam de trabalho de implementação têm o comentário a seguir. Certifique-se de adicionar/atualizar o código nesses arquivos com o código especificado neste tutorial.
>
>
>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>  &#x200B;>  // TODO: implemente isso seguindo as etapas do Tutorial do AEM Headless
>  &#x200B;>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>

## Anatomia do aplicativo React

A amostra do aplicativo React tem três partes principais que exigem atualização.

1. O arquivo `.env` contém a URL do serviço de Publicação (ou Visualização) do AEM.
1. O `src/components/Teams.js` exibe uma lista de equipes e seus membros.
1. O `src/components/Person.js` exibe os detalhes de um único membro da equipe.

## Implementar a funcionalidade de equipes

Crie a funcionalidade para exibir as Equipes e seus membros na exibição principal do aplicativo React. Essa funcionalidade exige:

* Um novo [gancho useEffect personalizado do React](https://react.dev/reference/react/useEffect#useeffect) que chama a **API Listar todos os Fragmentos de Conteúdo** por meio de uma solicitação de busca e, em seguida, obtém o valor `fullName` para cada `teamMember` para exibição.

Uma vez concluída, a visualização principal do aplicativo será preenchida com os dados das equipes do AEM.

![Exibição das equipes](./assets/4/teams.png)

1. Abra `src/components/Teams.js`.

1. Implemente o componente **Equipes** para buscar a lista de equipes da [API de Listar todos os Fragmentos de Conteúdo](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments) e renderizar o conteúdo das equipes. Isso é dividido nas seguintes etapas:

1. Crie um gancho `useEffect` que chame a API **List all Content Fragments** da AEM e armazene os dados no estado do componente React.
1. Para cada Fragmento de Conteúdo de **Equipe** retornado, chame a API **Obter um Fragmento de Conteúdo** para buscar os detalhes totalmente hidratados da equipe, incluindo seus membros e seus `fullNames`.
1. Renderize os dados das equipes usando a função `Team`.

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

## Implementar a funcionalidade de pessoa

Com a [funcionalidade Equipes](#implement-teams-functionality) concluída, implemente a funcionalidade para lidar com a exibição nos detalhes de um membro da equipe ou pessoa.

![Exibição de pessoa](./assets/4/person.png)

Para fazer isso:

1. Abrir `src/components/Person.js`
1. No componente React `Person`, analise o parâmetro de rota `id`. Observe que as Rotas do aplicativo React foram configuradas anteriormente para aceitar o parâmetro de URL `id` (consulte `/src/App.js`).
1. Busque os dados da pessoa no AEM para a [API de Obtenção de Fragmento de Conteúdo](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment).

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### Obter o código concluído

O código-fonte completo deste capítulo está [disponível em Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end).

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## Experimente o aplicativo

Revise o aplicativo [http://localhost:3000/](http://localhost:3000/) e clique nos links _Membro da equipe_. Além disso, você pode adicionar mais equipes e/ou membros ao Alpha da equipe, adicionando fragmentos de conteúdo no serviço de Autor do AEM e publicando-os.

## Nos bastidores

Abra o console **Ferramentas do Desenvolvedor > Rede** do navegador e **Filtro** para `/adobe/contentFragments` solicitações de busca enquanto você interage com o aplicativo React.

## Parabéns!

Parabéns! Você criou com sucesso um aplicativo React para consumir e exibir Fragmentos de conteúdo da entrega de fragmentos de conteúdo do AEM com APIs OpenAPI.
