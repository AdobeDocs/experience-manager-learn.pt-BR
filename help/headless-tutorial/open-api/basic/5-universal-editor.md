---
title: Tornar o aplicativo React editável com o Editor universal | Tutorial headless parte 5
description: Saiba como tornar seu aplicativo React editável no AEM Universal Editor adicionando a instrumentação e a configuração necessárias.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 800
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---


# Tornar o aplicativo React editável com o Editor universal

Neste capítulo, você aprenderá a criar o aplicativo React incorporado no [capítulo anterior](./4-react-app.md) editável usando o AEM Universal Editor. O Universal Editor permite que os autores de conteúdo editem o conteúdo diretamente no contexto da experiência do aplicativo React, mantendo a experiência contínua de um aplicativo headless.

![Editor Universal](./assets/5/main.png)

O Universal Editor fornece uma maneira poderosa de habilitar a edição em contexto para qualquer aplicativo web, permitindo que os autores editem o conteúdo sem alternar entre diferentes interfaces de criação.

## Pré-requisitos

* As etapas anteriores deste tutorial foram concluídas, especificamente [Criar um aplicativo React que usa OpenAPIs de entrega de fragmento de conteúdo do AEM](./4-react-app.md)
* Um conhecimento prático de [como usar e implementar o Universal Editor](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction).

## Objetivos

Saiba como:

* Adicione a instrumentação do Editor universal ao aplicativo React.
* Configure o aplicativo React para Editor universal.
* Ative a edição de conteúdo diretamente na interface do aplicativo React usando o Editor universal.

## Instrumentação do Editor universal

O Universal Editor requer [atributos HTML e metatags](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types) para identificar o conteúdo editável e estabelecer a conexão entre a interface do usuário e o conteúdo AEM.

### Adicionar tags do Editor universal

Primeiro, adicione as metatags necessárias para identificar o aplicativo React como compatível com o Universal Editor.

1. Abra o `public/index.html` no seu aplicativo React.
1. Adicione as [metatags do Editor Universal e o script CORS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/getting-started) na seção `<head>` do aplicativo React:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="utf-8" />
       <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
       <meta name="viewport" content="width=device-width, initial-scale=1" />
       <meta name="theme-color" content="#000000" />
       <meta name="description" content="WKND Teams React App" />
   
       <!-- Universal Editor meta tags and CORS script -->
       <meta name="urn:adobe:aue:system:aemconnection" content="aem:%REACT_APP_AEM_AUTHOR_HOST_URI%" />
       <script src="https://universal-editor-service.adobe.io/cors.js"></script>
   
       <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
       <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
       <title>WKND Teams</title>
   </head>
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="root"></div>
   </body>
   </html>
   ```

1. Atualize o arquivo `.env` do aplicativo React para incluir o host de serviço do Autor do AEM para oferecer suporte a write-backs no Editor Universal (usado no valor da tag de metat `urn:adobe:aue:system:aemconnection`).

   ```bash
   # The AEM Publish (or Preview) service
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   # The AEM Author service
   REACT_APP_AEM_AUTHOR_HOST_URI=https://author-p123-e456.adobeaemcloud.com
   ```

### Instrumentar o componente de equipes

Agora adicione atributos do Editor universal para tornar o componente Equipes editável.

1. Abra `src/components/Teams.js`.
1. Atualize o componente `Team` para incluir [atributos de dados do Editor Universal](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types):

   Ao definir o atributo `data-aue-resource`, verifique se o caminho AEM para o fragmento de conteúdo, conforme retornado pela entrega de fragmentos de conteúdo do AEM com APIs OpenAPI, é pós-fixado com o subcaminho para a variação de Fragmento de conteúdo; neste caso, `/jcr:content/data/master`.

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
               {...team}
           />
           );
       })}
       </div>
   );
   }
   
   /**
   * Team - renders a single team with its details and members
   * @param {Object} fields - The authored Content Fragment fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   * @param {string} path - Path of the team content fragment
   */
   function Team({ fields, references, path }) {
   if (!fields.title || !fields.teamMembers) {
       return null;
   }
   
   return (
       <>
       {/* Specify the correct Content Fragment variation path suffix in the data-aue-resource attribute */}
       <div className="team"
           data-aue-resource={`urn:aemconnection:${path}/jcr:content/data/master`}
           data-aue-type="component"
           data-aue-label={fields.title}>
   
           <h2 className="team__title"
           data-aue-prop="title"
           data-aue-type="text"
           data-aue-label="Team Title">{fields.title}</h2>
           <p className="team__description"
           data-aue-prop="description"
           data-aue-type="richtext"
           data-aue-label="Team Description"
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
           />
           <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {fields.teamMembers.map((teamMember, index) => {
               return (
                   <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                       {references[teamMember].value.fields.fullName}
                   </Link>
                   </li>
               );
               })}
           </ul>
           </div>
       </div>
       </>
   );
   }
   
   export default Teams;
   ```

### Instrumentar o componente de pessoa

Da mesma forma, adicione atributos do Editor universal ao componente Pessoa.

1. Abra `src/components/Person.js`.
1. Atualize o componente para incluir [atributos de dados do Editor Universal](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types):

   Ao definir o atributo `data-aue-resource`, verifique se o caminho AEM para o fragmento de conteúdo, conforme retornado pela entrega de fragmentos de conteúdo do AEM com APIs OpenAPI, é pós-fixado com o subcaminho para a variação de Fragmento de conteúdo; neste caso, `/jcr:content/data/master`.

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
       const { id } = useParams();
       const [person, setPerson] = useState(null);
   
       useEffect(() => {
           const fetchData = async () => {
           try {
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
       }, [id]);
   
       if (!person) {
           return <div>Loading person...</div>;
       }
   
       /* Add the Universal Editor data-aue-* attirbutes to the rendered HTML */
       return (
           <div className="person"
               data-aue-resource={`urn:aemconnection:${person.path}/jcr:content/data/master`}
               data-aue-type="component"
               data-aue-label={person.fields.fullName}>
               <img className="person__image"
                   src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
                   alt={person.fields.fullName}
                   data-aue-prop="profilePicture"
                   data-aue-type="media"
                   data-aue-label="Profile Picture"
               />
               <div className="person__occupations">
                   {person.fields.occupation.map((occupation, index) => {
                   return (
                       <span key={index} className="person__occupation">
                           {occupation}
                       </span>
                   );
                   })}
               </div>
   
               <div className="person__content">
                   <h1 className="person__full-name"
                       data-aue-prop="fullName"
                       data-aue-type="text"
                       data-aue-label="Full Name">
                       {person.fields.fullName}
                   </h1>
                   <div className="person__biography"
                       data-aue-prop="biographyText"
                       data-aue-type="richtext"
                       data-aue-label="Biography"
                       dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
                   />
               </div>
           </div>
       );
   }
   ```

### Obter o código concluído

O código-fonte completo deste capítulo está [disponível em Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end).


```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_5-end
```

## Testar integração com o Editor universal

Agora teste as atualizações de compatibilidade do Editor universal abrindo o aplicativo React no Editor universal.

### Iniciar o aplicativo React

1. Verifique se o aplicativo React está em execução:

   ```bash
   $ cd ~/Code/aem-guides-wknd-openapi/basic-tutorial
   $ npm install
   $ npm start
   ```

1. Verifique se o aplicativo é carregado em `http://localhost:3000` e exibe o conteúdo de equipes e pessoas.

### Executar proxy SSL local

O Universal Editor requer que o aplicativo editável seja carregado por HTTPS.

1. Para executar o aplicativo React local por HTTPS, use o módulo [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy) npm da linha de comando.

   ```bash
   $ npm install -g local-ssl-proxy
   $ local-ssl-proxy --source 3001 --target 3000
   ```

1. Abra `https://localhost:3001` no navegador da Web
1. Aceitar o certificado autoassinado.
1. Verifique se o aplicativo React é carregado.

### Abrir no Editor Universal

![Abrir aplicativo no Editor Universal](./assets/5/open-app-in-universal-editor.png)

1. Navegue até [Editor Universal](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/).
1. No campo **URL do Site**, insira a URL do aplicativo HTTPS React: `https://localhost:3001`.
1. Selecione Clique em **Abrir**.

O Editor universal deve carregar seu aplicativo React com os recursos de edição ativados.

### Testar funcionalidade de edição

![Editar no Editor Universal](./assets/5/edit-in-universal-editor.png)

1. No Editor universal, passe o mouse sobre os elementos editáveis no aplicativo React.

1. Para navegar no aplicativo React, ative e desative o modo **Visualização** novamente para editar. Lembre-se de que a **Visualização** não tem nada a ver com o serviço de Visualização do AEM. Em vez disso, ela ativa e desativa a edição do chrome no Universal Editor.

1. Você deve ver indicadores de edição e ser capaz de clicar nos vários elementos editáveis do aplicativo React.

1. Tente editar um título de equipe:
   * Clique no título de uma equipe
   * Editar o texto no painel de propriedades
   * Salve as alterações

1. Tente editar a foto do perfil de uma pessoa:
   * Clique na foto do perfil de uma pessoa
   * Selecione uma nova imagem no seletor de ativos
   * Salve as alterações

1. Pressione **Publicar** no canto superior direito do Universal Editor para publicar edições no serviço de Publicação (ou Visualização) do AEM, para que elas sejam refletidas no aplicativo React no Universal Editor.

## Atributos de dados do Editor Universal

Para obter a documentação completa sobre como instrumentar um aplicativo para o Universal Editor, consulte a [documentação do Universal Editor](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/).

## Parabéns!

Parabéns! Você integrou com êxito o Universal Editor ao seu aplicativo React. Os autores de conteúdo agora podem editar fragmentos de conteúdo diretamente na interface do aplicativo React, fornecendo uma experiência de criação contínua e mantendo os benefícios de uma arquitetura headless.

Lembre-se, você sempre pode obter o código-fonte final para este tutorial a partir da ramificação `main` do [repositório GitHub.com](https://github.com/adobe/aem-tutorials/tree/main).
