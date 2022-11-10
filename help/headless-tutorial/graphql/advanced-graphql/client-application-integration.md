---
title: Integração de aplicativos do cliente - Conceitos avançados de AEM headless - GraphQL
description: Implemente consultas persistentes e integre-as ao aplicativo WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# Integração de aplicativos do cliente

No capítulo anterior, você criou e atualizou consultas persistentes usando o Explorador GraphiQL.

Este capítulo o orienta pelas etapas para integrar as consultas persistentes ao aplicativo cliente WKND (também conhecido como Aplicativo WKND) usando solicitações HTTP GET dentro de **Reagir componentes**. Ela também oferece um desafio opcional para aplicar seus aprendizados sem cabeçalho de AEM, experiência em codificação para aprimorar o aplicativo cliente WKND.

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial de várias partes. Certifique-se de que os capítulos anteriores foram concluídos antes de prosseguir com este capítulo. O aplicativo cliente WKND se conecta AEM serviço de publicação, portanto, é importante que você **publicou o seguinte no serviço de publicação de AEM**.

* Configurações do projeto
* Pontos de extremidade GraphQL.
* Modelos de fragmentos do conteúdo
* Fragmentos de conteúdo autoral
* Consultas persistentes de GraphQL

O _As capturas de tela do IDE neste capítulo vêm de [Código do Visual Studio](https://code.visualstudio.com/)_

### Capítulo 1-4 Pacote de soluções (opcional) {#solution-package}

Um pacote de soluções está disponível para ser instalado e conclui as etapas na interface do usuário do AEM para os capítulos 1 a 4. Este pacote é **não é necessário** se os capítulos anteriores tiverem sido concluídos.

1. Baixar [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. Em AEM, navegue até **Ferramentas** > **Implantação** > **Pacotes** para acessar **Gerenciador de pacotes**.
1. Faça upload e instale o pacote (arquivo zip) baixado na etapa anterior.
1. Replicar o pacote para o serviço de publicação do AEM

## Objetivos {#objectives}

Neste tutorial, você aprenderá a integrar as solicitações de consultas persistentes ao aplicativo WKND GraphQL React de amostra usando o [Cliente autônomo do AEM para JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clonar e executar o aplicativo cliente de amostra {#clone-client-app}

Para acelerar o tutorial, é fornecido um aplicativo React JS inicial.

1. Clonar o [adobe/aem-guides-wknd-grafql](https://github.com/adobe/aem-guides-wknd-graphql) repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd-graphql/advanced-tutorial/.env.development` arquivo e conjunto `REACT_APP_HOST_URI` para apontar para o serviço de publicação do target AEM.

   Atualize o método de autenticação se estiver se conectando a uma instância do autor.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![React App Development Environment](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > As instruções acima são para conectar o aplicativo React ao **Serviço de publicação do AEM**, no entanto para se conectar ao **Serviço de criação do AEM** obtenha um token de desenvolvimento local para seu ambiente de destino AEM as a Cloud Service.
   >
   > Também é possível conectar o aplicativo a um [instância de autor local usando o SDK do AEMaaCS](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) usando autenticação básica.


1. Abra um terminal e execute os comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador deve ser carregada em [http://localhost:3000](http://localhost:3000)


1. Toque **Acampamento** > **Embalagem de origem Yosemite** para exibir os detalhes da aventura de Backpack do Yosemite.

   ![Tela de Backpack do Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Abra as ferramentas do desenvolvedor do navegador e inspecione o `XHR` solicitação

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Você deveria ver `GET` solicitações para o ponto de extremidade GraphQL com o nome de configuração do projeto (`wknd-shared`), nome de consulta persistente (`adventure-by-slug`), nome da variável (`slug`), valor (`yosemite-backpacking`) e codificações de caracteres especiais.

>[!IMPORTANT]
>
>    Se você estiver se perguntando por que a solicitação da API GraphQL é feita em relação à variável `http://localhost:3000` e NÃO no domínio do AEM Publish Service, revise [Sob O Capuz](../multi-step/graphql-and-react-app.md#under-the-hood) em Tutorial básico.


## Revise o código

No [Tutorial básico - Crie um aplicativo React que use APIs GraphQL AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) o passo que tínhamos revisado e aprimorado alguns arquivos principais para obter experiência prática. Antes de aprimorar o aplicativo WKND, analise os arquivos principais.

* [Revise o objeto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementar para executar AEM consultas persistentes de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Revisão `Adventures` Componente de reação

A exibição principal do aplicativo WKND React é a lista de todas as Aventuras e você pode filtrá-las com base no tipo de atividade como _Acampamento, Ciclo_. Essa exibição é renderizada pela variável `Adventures` componente. Abaixo estão os principais detalhes de implementação:

* O `src/components/Adventures.js` chamadas `useAllAdventures(adventureActivity)` gancho e aqui `adventureActivity` argumento é tipo de atividade.

* O `useAllAdventures(adventureActivity)` o gancho é definido na variável `src/api/usePersistedQueries.js` arquivo. Baseado em `adventureActivity` determina qual consulta persistiu chamar. Se não for um valor nulo, ele chamará `wknd-shared/adventures-by-activity`, o restante obtém todas as aventuras disponíveis `wknd-shared/adventures-all`.

* O gancho usa o principal `fetchPersistedQuery(..)` que delega a execução da consulta a `AEMHeadless` via `aemHeadlessClient.js`.

* O gancho também retorna somente os dados relevantes da resposta GraphQL da AEM em `response.data?.adventureList?.items`, permitindo que o `Adventures` React view components para serem agnósticos das estruturas JSON pai.

* Após a execução bem-sucedida do query, a variável `AdventureListItem(..)` função de renderização de `Adventures.js` adiciona elemento HTML para exibir o _Imagem, Extensão do Percurso, Preço e Título_ informações.

### Revisão `AdventureDetail` Componente de reação

O `AdventureDetail` O componente React renderiza os detalhes da aventura. Abaixo estão os principais detalhes de implementação:

* O `src/components/AdventureDetail.js` chamadas `useAdventureBySlug(slug)` gancho e aqui `slug` O argumento é parâmetro de consulta.

* Como acima, a variável `useAdventureBySlug(slug)` o gancho é definido na variável `src/api/usePersistedQueries.js` arquivo. Chama `wknd-shared/adventure-by-slug` consulta persistente delegando a `AEMHeadless` via `aemHeadlessClient.js`.

* Após a execução bem-sucedida do query, a variável `AdventureDetailRender(..)` função de renderização de `AdventureDetail.js` adiciona o elemento HTML para exibir os detalhes da Aventura.


## Aprimoramento do código

### Use `adventure-details-by-slug` consulta persistente

No capítulo anterior, criamos o `adventure-details-by-slug` consulta persistente, ela fornece informações adicionais do Adventure, como _localização, instrutorTeam e administrador_. Vamos substituir `adventure-by-slug` com `adventure-details-by-slug` consulta persistente para renderizar essas informações adicionais.

1. Abrir `src/api/usePersistedQueries.js`.

1. Localize a função `useAdventureBySlug()` e atualizar consulta como

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Exibir informações adicionais

1. Para exibir informações adicionais de aventura, abra `src/components/AdventureDetail.js`

1. Localize a função `AdventureDetailRender(..)` e atualizar a função de retorno como

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. Defina também as funções de renderização correspondentes:

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **Local**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstrutorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **Administrador**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### Definir novos estilos

1. Abrir `src/components/AdventureDetail.scss` e adicionar as seguintes definições de classe

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>Os arquivos atualizados estão disponíveis em **WKND de guias de AEM - GraphQL** projeto, consulte [Tutorial avançado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) seção.


Após concluir os aprimoramentos acima, o aplicativo WKND tem a aparência abaixo e as ferramentas de desenvolvedor do navegador mostram `adventure-details-by-slug` chamada de consulta persistente.

![APLICATIVO WKND aprimorado](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Desafio de aprimoramento (opcional)

A exibição principal do aplicativo WKND React permite filtrar essas Aventuras com base no tipo de atividade como _Acampamento, Ciclo_. No entanto, a equipe de negócios da WKND deseja ter um extra _Localização_ recurso de filtragem baseado. Os requisitos são

* Na exibição principal do aplicativo WKND, no canto superior esquerdo ou direito, adicione _Localização_ ícone do filtro.
* Clicar _Localização_ ícone de filtragem deve exibir a lista de locais.
* Clicar em uma opção de local desejada na lista só deve mostrar as Aventuras correspondentes.
* Se houver apenas uma Aventura correspondente, a exibição Detalhes da Aventura será mostrada.

## Parabéns

Parabéns. Agora você concluiu a integração e a implementação das consultas persistentes no aplicativo WKND de amostra.
