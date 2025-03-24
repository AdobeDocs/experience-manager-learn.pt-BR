---
title: Integração de aplicativos clientes - Conceitos avançados do AEM Headless - GraphQL
description: Implemente consultas persistentes e integre-as ao aplicativo WKND.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 241
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 1%

---

# Integração de aplicativos cliente

No capítulo anterior, você criou e atualizou consultas persistentes usando o GraphiQL Explorer.

Este capítulo percorre as etapas para integrar as consultas persistentes ao aplicativo cliente WKND (também conhecido como Aplicativo WKND) usando solicitações HTTP GET em **componentes React** existentes. Ele também oferece um desafio opcional para aplicar seus aprendizados do AEM Headless, experiência em codificação para aprimorar o aplicativo cliente WKND.

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial dividido em várias partes. Assegure-se de que os capítulos anteriores foram completados antes de prosseguir com este capítulo. O aplicativo cliente WKND conecta-se ao serviço de publicação do AEM, portanto, é importante que você **tenha publicado o seguinte no serviço de publicação do AEM**.

* Configurações do projeto
* Pontos de extremidade GraphQL.
* Modelos de fragmentos do conteúdo
* Fragmentos de conteúdo criados
* Consultas persistentes do GraphQL

As _capturas de tela do IDE neste capítulo vêm do [Visual Studio Code](https://code.visualstudio.com/)_

### Capítulo 1-4 Pacote de soluções (opcional) {#solution-package}

Um pacote de solução está disponível para instalação e conclui as etapas na interface do usuário do AEM para os capítulos 1 a 4. Este pacote é **não necessário** se os capítulos anteriores foram concluídos.

1. Baixar [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. No AEM, navegue até **Ferramentas** > **Implantação** > **Pacotes** para acessar o **Gerenciador de Pacotes**.
1. Carregue e instale o pacote (arquivo zip) baixado na etapa anterior.
1. Replicar o pacote para o serviço de publicação do AEM

## Objetivos {#objectives}

Neste tutorial, você aprenderá a integrar as solicitações de consultas persistentes no aplicativo WKND GraphQL React de exemplo usando o [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js).

## Clonar e executar o aplicativo cliente de amostra {#clone-client-app}

Para acelerar o tutorial, é fornecido um aplicativo JS React de início.

1. Clonar o repositório [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite o arquivo `aem-guides-wknd-graphql/advanced-tutorial/.env.development` e defina `REACT_APP_HOST_URI` para apontar para o serviço de publicação do AEM de destino.

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
   > As instruções acima são para conectar o aplicativo React ao **serviço de Publicação do AEM**. No entanto, para se conectar ao **serviço de Autor do AEM**, obtenha um token de desenvolvimento local para o ambiente do AEM as a Cloud Service de destino.
   >
   > Também é possível conectar o aplicativo a uma [instância de Autor local usando o AEMaaCS SDK](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) com autenticação básica.


1. Abra um terminal e execute os comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Uma nova janela de navegador deve ser carregada em [http://localhost:3000](http://localhost:3000)


1. Toque em **Camping** > **Yosemite Backpacking** para exibir os detalhes da aventura do Yosemite Backpacking.

   ![Tela de Mochila de Yosemite](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Abra as ferramentas de desenvolvedor do navegador e inspecione a solicitação `XHR`

   ![POSTAR GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   Você deve ver `GET` solicitações para o ponto de extremidade GraphQL com nome de configuração de projeto (`wknd-shared`), nome de consulta persistente (`adventure-by-slug`), nome da variável (`slug`), valor (`yosemite-backpacking`) e codificações de caracteres especiais.

>[!IMPORTANT]
>
>    Se você estiver se perguntando por que a solicitação da API do GraphQL é feita em relação ao `http://localhost:3000` e NÃO em relação ao domínio do Serviço de publicação do AEM, revise [Por baixo](../multi-step/graphql-and-react-app.md#under-the-hood) no Tutorial básico.


## Revise o código

No [Tutorial básico - Criar um aplicativo React que usa as APIs GraphQL da AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object), revisamos e aprimoramos alguns arquivos importantes para obter experiência prática. Antes de aprimorar o aplicativo WKND, reveja os arquivos principais.

* [Revise o objeto AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [Implementar para executar consultas persistentes do AEM GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### Revisar Componente de Reação `Adventures`

A exibição principal do aplicativo WKND React é a lista de todas as Aventuras e você pode filtrar essas Aventuras com base no tipo de atividade como _Camping, Cycling_. Esta exibição é renderizada pelo componente `Adventures`. Abaixo estão os principais detalhes de implementação:

* O `src/components/Adventures.js` chama o gancho `useAllAdventures(adventureActivity)` e aqui o argumento `adventureActivity` é o tipo de atividade.

* O gancho `useAllAdventures(adventureActivity)` está definido no arquivo `src/api/usePersistedQueries.js`. Com base no valor `adventureActivity`, ele determina qual consulta persistente chamar. Se não for um valor nulo, ele chamará `wknd-shared/adventures-by-activity`; caso contrário, obterá todas as aventuras disponíveis `wknd-shared/adventures-all`.

* O gancho usa a função principal `fetchPersistedQuery(..)` que delega a execução da consulta para `AEMHeadless` via `aemHeadlessClient.js`.

* O gancho também retorna apenas os dados relevantes da resposta do AEM GraphQL em `response.data?.adventureList?.items`, permitindo que os componentes da exibição do React `Adventures` sejam agnósticos das estruturas JSON principais.

* Após a execução bem-sucedida da consulta, a função de renderização `AdventureListItem(..)` de `Adventures.js` adiciona o elemento HTML para exibir as informações de _Imagem, Duração da Viagem, Preço e Título_.

### Revisar Componente de Reação `AdventureDetail`

O componente React `AdventureDetail` renderiza os detalhes da aventura. Abaixo estão os principais detalhes de implementação:

* O `src/components/AdventureDetail.js` chama o gancho `useAdventureBySlug(slug)` e aqui o argumento `slug` é o parâmetro de consulta.

* Como acima, o gancho `useAdventureBySlug(slug)` é definido no arquivo `src/api/usePersistedQueries.js`. Ele chama a consulta persistente `wknd-shared/adventure-by-slug` delegando a `AEMHeadless` via `aemHeadlessClient.js`.

* Após a execução bem-sucedida da consulta, a função de renderização `AdventureDetailRender(..)` de `AdventureDetail.js` adiciona o elemento HTML para exibir os detalhes de Adventure.


## Aprimorar o código

### Usar consulta persistente `adventure-details-by-slug`

No capítulo anterior, criamos a consulta persistente `adventure-details-by-slug`. Ela fornece informações adicionais do Adventure, como _local, professorEquipe e administrador_. Vamos substituir `adventure-by-slug` por `adventure-details-by-slug` consulta persistente para renderizar essas informações adicionais.

1. Abrir `src/api/usePersistedQueries.js`.

1. Localize a função `useAdventureBySlug()` e atualize a consulta como

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

1. Localize a função `AdventureDetailRender(..)` e atualize a função de retorno como

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

   **InfoLocal**

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

   **EquipeInstrutora**

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
>Os arquivos atualizados estão disponíveis no projeto **WKND do AEM Guides - GraphQL**, consulte a seção [Tutorial avançado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial).


Após concluir as melhorias acima, o aplicativo WKND se parece com o mostrado abaixo e as ferramentas de desenvolvedor do navegador mostram `adventure-details-by-slug` chamada de consulta persistente.

![APLICATIVO WKND aprimorado](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Desafio de aprimoramento (opcional)

A exibição principal do aplicativo WKND React permite filtrar essas Aventuras com base no tipo de atividade como _Acampamento, Ciclismo_. No entanto, a equipe de negócios da WKND deseja ter um recurso de filtragem adicional baseado em _Local_. Os requisitos são

* Na exibição principal do Aplicativo WKND, no canto superior esquerdo ou direito, adicione o ícone de filtragem _Local_.
* Clicar no ícone de filtragem _Local_ deve exibir uma lista de locais.
* Clicar em uma opção de localização desejada na lista só deve mostrar Aventuras correspondentes.
* Se houver apenas uma Aventura correspondente, a exibição Detalhes da Aventura será exibida.

## Parabéns

Parabéns! Agora você concluiu a integração e a implementação das consultas persistentes no aplicativo WKND de amostra.
