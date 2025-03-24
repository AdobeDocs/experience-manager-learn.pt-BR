---
title: Componente web/JS - Exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Web Component/JS demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 1%

---

# Componente da Web

Aplicativos de exemplo são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Web Component demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes e renderizar uma parte da interface do usuário, realizada usando código puro JavaScript.

![Componente da Web com AEM Headless](./assets/web-component/web-component.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O componente Web funciona com as seguintes opções de implantação do AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuração local usando o [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
   + Requer o [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (se estiver se conectando ao AEM 6.5 ou AEM SDK local)

Este aplicativo de exemplo depende da [basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip) para ser instalado e das [configurações de implantação](../deployment/web-component.md) necessárias estão em vigor.


>[!IMPORTANT]
>
>O componente da Web foi projetado para se conectar a um ambiente de __Publicação do AEM__. No entanto, ele poderá originar conteúdo do Autor do AEM se a autenticação for fornecida no arquivo [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) do componente da Web.

## Como usar

1. Clonar o repositório `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navegue até o subdiretório `web-component`.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Edite o arquivo `.../src/person.js` para incluir os detalhes de conexão do AEM:

   No objeto `aemHeadlessService`, atualize o `aemHost` para apontar para o serviço de Publicação do AEM.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Se estiver se conectando a um serviço do AEM Author, no objeto `aemCredentials`, forneça credenciais de usuário locais do AEM.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Abra um terminal e execute os comandos de `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador abre a página estática do HTML que incorpora o componente Web em [http://localhost:8080](http://localhost:8080).
1. O componente Web _Informações sobre a pessoa_ é exibido na página da Web.

## O código

Abaixo está um resumo de como o componente da Web é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Tag HTML do componente da Web

Um Componente Web reutilizável (também conhecido como elemento personalizado) `<person-info>` foi adicionado à página do HTML `../src/assets/aem-headless.html`. Ele oferece suporte aos atributos `host` e `query-param-value` para direcionar o comportamento do componente. O valor do atributo `host` substitui o valor `aemHost` do objeto `aemHeadlessService` em `person.js` e `query-param-value` é usado para selecionar a pessoa a ser renderizada.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementação do componente da Web

O `person.js` define a funcionalidade do Componente Web e abaixo estão os principais destaques dele.

#### Implementação do elemento PersonInfo

O objeto de classe do elemento personalizado `<person-info>` define a funcionalidade usando os métodos de ciclo de vida `connectedCallback()`, anexando uma raiz de sombra, buscando consulta persistente do GraphQL e manipulação de DOM para criar a estrutura DOM de sombra interna do elemento personalizado.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registrar o elemento `<person-info>`

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Compartilhamento de recursos entre origens (CORS)

Este componente da Web depende de uma configuração do CORS com base em AEM executada no ambiente do AEM de destino e presume que a página de host é executada em `http://localhost:8080` no modo de desenvolvimento e abaixo está um exemplo de configuração do CORS OSGi para o serviço de autor local do AEM.

Revise as [configurações de implantação](../deployment/web-component.md) para os respectivos serviços da AEM.
