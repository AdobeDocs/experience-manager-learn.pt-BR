---
title: Componente da Web/JS - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Web Component/JS demonstra como consultar conteúdo usando APIs AEM GraphQL usando consultas persistentes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 7%

---


# Componente Web

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Esse aplicativo de componente da Web demonstra como consultar o conteúdo usando AEM APIs do GraphQL usando consultas persistentes e renderizar uma parte da interface do usuário, realizada usando código JavaScript puro.

![Componente da Web com AEM headless](./assets/web-component/web-component.png)

Visualize o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (se estiver se conectando ao AEM local 6.5 ou AEM SDK)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Requisitos AEM

O Web Component funciona com as seguintes opções de implantação de AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=pt-BR)
+ Configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR?lang=en#install-local-aem-instances)

Todas as implantações exigem o `tutorial-solution-content.zip` do [Arquivos de solução](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) a instalar e a [Configurações de implantação](../deployment/web-component.md) são realizadas.


>[!IMPORTANT]
>
>O componente Web foi projetado para se conectar a um __Publicação do AEM__ , no entanto, ele pode originar conteúdo do Autor do AEM se a autenticação for fornecida no [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) arquivo.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navegar para `web-component` subdiretório.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Edite o `.../src/person.js` para incluir os detalhes da conexão AEM:

   No `aemHeadlessService` , atualize o `aemHost` para apontar para seu serviço de publicação do AEM.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Se estiver se conectando a um serviço de Autor do AEM, na `aemCredentials` , forneça credenciais de usuário do AEM local.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Abra um terminal e execute os comandos do `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador abre a página HTML estática que incorpora o Componente Web em [http://localhost:8080](http://localhost:8080).
1. O _Informações da pessoa_ O Web Component é exibido na página da Web.

## O código

Abaixo está um resumo de como o Componente Web é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Tag HTML do componente Web

Um componente web reutilizável (também conhecido como elemento personalizado) `<person-info>` é adicionado ao `../src/assets/aem-headless.html` HTML. É compatível `host` e `query-param-value` para direcionar o comportamento do componente. O `host` substituições de valor do atributo `aemHost` valor de `aemHeadlessService` em `person.js`e `query-param-value` é usada para selecionar a pessoa a ser renderizada.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementação de componentes da Web

O `person.js` define a funcionalidade do Componente da Web e abaixo estão os principais destaques.

#### Implementação do elemento PersonInfo

O `<person-info>` o objeto de classe do elemento personalizado define a funcionalidade usando o `connectedCallback()` métodos do ciclo de vida, anexando uma raiz de sombra, buscando a consulta persistente do GraphQL e a manipulação de DOM para criar a estrutura interna de DOM de sombra do elemento personalizado.

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
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registre o `<person-info>` elemento

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Compartilhamento de recursos entre origens (CORS)

Esse Componente Web depende de uma configuração de CORS baseada em AEM em execução no ambiente de AEM de destino e presume que a página de host seja executada em `http://localhost:8080` no modo de desenvolvimento e abaixo está uma amostra da configuração OSGi do CORS para o serviço AEM Author local.

Revise [configurações de implantação](../deployment/web-component.md) para o respectivo serviço AEM.

![Configuração do CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
