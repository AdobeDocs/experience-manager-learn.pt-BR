---
title: Integração de aplicativos clientes - Conceitos avançados do AEM Headless - GraphQL
description: Implemente consultas persistentes e integre-as ao aplicativo WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 290
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 1%

---

# Integração de aplicativos cliente

No capítulo anterior, você criou e atualizou consultas persistentes usando o GraphiQL Explorer.

Este capítulo percorre as etapas para integrar as consultas persistentes ao aplicativo cliente WKND (também conhecido como Aplicativo WKND) usando solicitações HTTP GET em aplicativos existentes **Componentes do React**. Ele também oferece um desafio opcional para aplicar seus aprendizados AEM Headless, experiência em codificação para aprimorar o aplicativo cliente WKND.

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial dividido em várias partes. Assegure-se de que os capítulos anteriores foram completados antes de prosseguir com este capítulo. O aplicativo cliente WKND se conecta ao serviço de publicação AEM, portanto, é importante que você **publicado o seguinte no serviço de publicação do AEM**.

* Configurações do projeto
* Pontos de extremidade GraphQL.
* Modelos de fragmentos do conteúdo
* Fragmentos de conteúdo criados
* Consultas persistentes do GraphQL

A variável _As capturas de tela do IDE neste capítulo vêm de [Código do Visual Studio](https://code.visualstudio.com/)_

### Capítulo 1-4 Pacote de soluções (opcional) {#solution-package}

Um pacote de solução está disponível para ser instalado e conclui as etapas na interface do usuário do AEM para os capítulos 1 a 4. Este pacote é **não é necessário** se os capítulos anteriores tiverem sido concluídos.

1. Baixar [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. No AEM, navegue até **Ferramentas** > **Implantação** > **Pacotes** para acessar **Gerenciador de pacotes**.
1. Carregue e instale o pacote (arquivo zip) baixado na etapa anterior.
1. Replicar o pacote para o serviço de publicação do AEM

## Objetivos {#objectives}

Neste tutorial, você aprenderá a integrar as solicitações de consultas persistentes no aplicativo WKND GraphQL React de amostra usando o [Cliente AEM Headless para JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clonar e executar o aplicativo cliente de amostra {#clone-client-app}

Para acelerar o tutorial, é fornecido um aplicativo JS React de início.

1. Clonar o [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o `aem-guides-wknd-graphql/advanced-tutorial/.env.development` arquivo e definir `REACT_APP_HOST_URI` para apontar para o serviço de publicação do AEM no target.

   Atualize o método de autenticação se estiver se conectando a uma instância de autor.

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

   ![Ambiente de desenvolvimento do aplicativo React](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > As instruções acima são para conectar o aplicativo React ao **Serviço de publicação do AEM**, no entanto, para se conectar ao **Serviço de Autor do AEM** obtenha um token de desenvolvimento local para o ambiente as a Cloud Service do AEM de destino.
   >
   > Também é possível conectar o aplicativo a um [Instância de autor local usando o SDK do AEMaaCS](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) usando autenticação básica.


1. Abra um terminal e execute os comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador deve ser carregada em [http://localhost:3000](http://localhost:3000)


1. Toque **Camping** > **Mochila de Yosemite** para ver os detalhes da aventura do Yosemite Backpacking.

   ![Tela de mochila Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Abra as ferramentas de desenvolvedor do navegador e inspecione as `XHR` solicitação

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Você deve ver `GET` solicitações para o endpoint do GraphQL com o nome de configuração do projeto (`wknd-shared`), nome da consulta persistente (`adventure-by-slug`), nome da variável (`slug`), valor (`yosemite-backpacking`) e codificações de caracteres especiais.

>[!IMPORTANT]
>
>    Se você estiver se perguntando por que a solicitação da API do GraphQL é feita em relação ao `http://localhost:3000` e NÃO contra o domínio AEM Publish Service, revise [Debaixo da tampa](../multi-step/graphql-and-react-app.md#under-the-hood) do Tutorial básico.


## Revise o código

No [Tutorial básico - Criar um aplicativo React que usa APIs AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) etapa revisamos e aprimoramos alguns arquivos importantes para obter experiência prática. Antes de aprimorar o aplicativo WKND, reveja os arquivos principais.

* [Revisar o objeto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementar para executar consultas persistentes do AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Revisão `Adventures` Componente do React

A visualização principal do aplicativo WKND React é a lista de todas as Aventuras e você pode filtrar essas Aventuras com base no tipo de atividade como _Acampamento, Ciclismo_. Esta exibição é renderizada pelo `Adventures` componente. Abaixo estão os principais detalhes de implementação:

* A variável `src/components/Adventures.js` chamadas `useAllAdventures(adventureActivity)` gancho e aqui `adventureActivity` o argumento é tipo de atividade.

* A variável `useAllAdventures(adventureActivity)` o gancho é definido na variável `src/api/usePersistedQueries.js` arquivo. Baseado em `adventureActivity` , determina qual consulta persistente chamar. Se não for um valor nulo, ele chamará `wknd-shared/adventures-by-activity`, senão obterá todas as aventuras disponíveis `wknd-shared/adventures-all`.

* O gancho usa a variável principal `fetchPersistedQuery(..)` função que delega a execução da consulta a `AEMHeadless` via `aemHeadlessClient.js`.

* O gancho também retorna apenas os dados relevantes da resposta do AEM GraphQL em `response.data?.adventureList?.items`, permitindo que o `Adventures` Os componentes de visualização do React são agnósticos das estruturas JSON principais.

* Após a execução bem-sucedida da consulta, a variável `AdventureListItem(..)` função de renderização de `Adventures.js` adiciona o elemento HTML para exibir o _Imagem, Duração do Percurso, Preço e Título_ informações.

### Revisão `AdventureDetail` Componente do React

A variável `AdventureDetail` O componente React renderiza os detalhes da aventura. Abaixo estão os principais detalhes de implementação:

* A variável `src/components/AdventureDetail.js` chamadas `useAdventureBySlug(slug)` gancho e aqui `slug` argumento é parâmetro de consulta.

* Como acima, a variável `useAdventureBySlug(slug)` o gancho é definido na variável `src/api/usePersistedQueries.js` arquivo. Chama `wknd-shared/adventure-by-slug` consulta persistente delegando a `AEMHeadless` via `aemHeadlessClient.js`.

* Após a execução bem-sucedida da consulta, a variável `AdventureDetailRender(..)` função de renderização de `AdventureDetail.js` adiciona o elemento HTML para exibir os detalhes de Aventura.


## Aprimorar o código

### Uso `adventure-details-by-slug` consulta persistente

No capítulo anterior, criamos a variável `adventure-details-by-slug` consulta persistente, ele fornece informações adicionais do Adventure, como _local, equipe de instrutores e administrador_. Vamos substituir `adventure-by-slug` com `adventure-details-by-slug` consulta persistente para renderizar essas informações adicionais.

1. Abertura `src/api/usePersistedQueries.js`.

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

1. Para exibir informações adicionais sobre aventura, abra `src/components/AdventureDetail.js`

1. Localize a função `AdventureDetailRender(..)` e atualizar função de retorno como

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

   **InstructorTeam**

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

1. Abertura `src/components/AdventureDetail.scss` e adicione as seguintes definições de classe

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
>Os arquivos atualizados estão disponíveis em **Guias de AEM WKND - GraphQL** projeto, consulte [Tutorial avançado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) seção.


Depois de concluir as melhorias acima, o aplicativo WKND se parece com o mostrado abaixo e as ferramentas do desenvolvedor do navegador são exibidas `adventure-details-by-slug` chamada de consulta persistente.

![APLICATIVO WKND avançado](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Desafio de aprimoramento (opcional)

A visualização principal do aplicativo WKND React permite filtrar essas Aventuras com base no tipo de atividade, como _Acampamento, Ciclismo_. No entanto, a equipe de negócios da WKND deseja ter uma _Localização_ recurso de filtragem baseado em. Os requisitos são

* Na visualização principal do aplicativo WKND, no canto superior esquerdo ou direito, adicione _Localização_ ícone de filtragem.
* Clicando _Localização_ o ícone de filtragem deve exibir a lista de locais.
* Clicar em uma opção de localização desejada na lista só deve mostrar Aventuras correspondentes.
* Se houver apenas uma Aventura correspondente, a exibição Detalhes da Aventura será exibida.

## Parabéns

Parabéns! Agora você concluiu a integração e a implementação das consultas persistentes no aplicativo WKND de amostra.
