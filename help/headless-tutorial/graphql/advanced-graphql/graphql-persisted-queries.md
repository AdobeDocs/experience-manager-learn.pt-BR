---
title: Consultas GraphQL persistentes - Conceitos avançados de AEM sem cabeçalho - GraphQL
description: Neste capítulo de Conceitos avançados do Adobe Experience Manager (AEM) Headless, saiba como criar e atualizar consultas GraphQL persistentes com parâmetros. Saiba como transmitir parâmetros de controle de cache em consultas persistentes.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# Consultas persistentes de GraphQL 

As consultas persistentes são consultas armazenadas no servidor do Adobe Experience Manager (AEM). Os clientes podem enviar uma solicitação HTTP GET com o nome da consulta para executá-la. O benefício desta abordagem é a capacidade de armazenamento em cache. Embora as consultas GraphQL do lado do cliente também possam ser executadas usando solicitações HTTP POST, que não podem ser armazenadas em cache, as consultas persistentes podem ser armazenadas em cache por caches HTTP ou CDN, melhorando o desempenho. As consultas persistentes permitem simplificar as solicitações e melhorar a segurança, pois as consultas são encapsuladas no servidor e o administrador do AEM tem controle total sobre elas. É **práticas recomendadas e altamente recomendadas** para usar consultas persistentes ao trabalhar com a API GraphQL AEM.

No capítulo anterior, você explorou algumas consultas GraphQL avançadas para coletar dados do aplicativo WKND. Neste capítulo, você mantém as consultas para AEM e aprende a usar o controle de cache em consultas persistentes.

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial de várias partes. Certifique-se de que [capítulo anterior](explore-graphql-api.md) foi concluída antes de prosseguir com este capítulo.

## Objetivos {#objectives}

Neste capítulo, saiba como:

* Manter consultas GraphQL com parâmetros
* Usar parâmetros de controle de cache com consultas persistentes

## Revisão _Consultas Persistentes GraphQL_ configuração

Vamos revisar isso _Consultas Persistentes GraphQL_ são ativadas para o projeto do Site WKND na sua instância de AEM.

1. Navegar para **Ferramentas** > **Geral** > **Navegador de configuração**.

1. Selecionar **WKND Compartilhado**, em seguida selecione **Propriedades** na barra de navegação superior para abrir as propriedades de configuração. Na página Propriedades de configuração , você deve ver que a variável **Consultas Persistentes GraphQL** está ativada.

   ![Propriedades de configuração](assets/graphql-persisted-queries/configuration-properties.png)

## Mantenha consultas GraphQL usando a ferramenta GraphiQL Explorer incorporada

Nesta seção, vamos continuar com a consulta GraphQL usada posteriormente no aplicativo cliente para buscar e renderizar os dados do Fragmento de conteúdo da Aventura.

1. Insira a seguinte query no explorador GraphiQL:

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   Verifique se a consulta funciona antes de salvá-la.

1. Próximo toque em Salvar como e insira `adventure-details-by-slug` como o Nome da consulta.

   ![Persistir consulta GraphQL](assets/graphql-persisted-queries/persist-graphql-query.png)

## Executando query persistente com variáveis codificando caracteres especiais

Vamos entender como as consultas persistentes com variáveis são executadas pelo aplicativo do lado do cliente, codificando os caracteres especiais.

Para executar uma consulta persistente, o aplicativo cliente faz uma solicitação do GET usando a seguinte sintaxe:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Para executar uma consulta persistente _com uma variável_, a sintaxe acima é alterada para:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

Os caracteres especiais, como ponto e vírgula (;), sinal de igual (=), barras (/) e espaço devem ser convertidos para usar a codificação UTF-8 correspondente.

Ao executar o `getAllAdventureDetailsBySlug` do terminal de linha de comando, revisamos esses conceitos em ação.

1. Abra o navegador GraphiQL e clique no link **elipses** (...) ao lado da consulta persistente `getAllAdventureDetailsBySlug`, depois clique em **Copiar URL**. Cole o URL copiado em um bloco de texto; a aparência abaixo é:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Adicionar `yosemite-backpacking` como valor variável

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Codifique os pontos e vírgulas (;) e o sinal de igual (=) caracteres especiais

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Abra um terminal de linha de comando e use [Curl](https://curl.se/) executar a consulta

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Se estiver executando a consulta acima no ambiente do autor do AEM, você deverá enviar as credenciais. Consulte [Token de acesso de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) para a sua demonstração e [Chamar a API AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) para obter detalhes sobre a documentação.

Além disso, revise [Como executar uma consulta Persistente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Uso de variáveis de consulta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables)e [Codificação do URL de consulta para uso por um aplicativo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) para saber mais sobre a execução persistente do query por aplicativos clientes.

## Atualizar parâmetros de controle de cache em consultas persistentes {#cache-control-all-adventures}

A API GraphQL da AEM permite atualizar os parâmetros de controle de cache padrão para suas consultas a fim de melhorar o desempenho. Os valores padrão de controle de cache são:

* 60 segundos é o TTL padrão (maxage=60) para o cliente (por exemplo, um navegador)

* 7200 segundos é o TTL padrão (s-maxage=7200) para o Dispatcher e CDN; também conhecidos como caches compartilhados

Use o `adventures-all` para atualizar os parâmetros de controle de cache. A resposta da consulta é grande e é útil controlar sua `age` no cache. Essa consulta persistente é usada posteriormente para atualizar o [aplicativo cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Abra o navegador GraphiQL e clique no link **elipses** (...) ao lado da consulta persistente, em seguida, clique em **Cabeçalhos** para abrir **Configuração de cache** modal.

   ![Opção Persistir Cabeçalho GraphQL](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. No **Configuração de cache** modal, atualize o `max-age` valor do cabeçalho para `600 `segundos (10 minutos) e clique em **Salvar**

   ![Persistir configuração do cache GraphQL](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Revisão [Armazenamento em cache de suas consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) para obter mais informações sobre os parâmetros padrão de controle de cache.


## Parabéns. 

Parabéns. Agora você aprendeu a manter consultas GraphQL com parâmetros, atualizar consultas persistentes e usar parâmetros de controle de cache com consultas persistentes.

## Próximas etapas

No [próximo capítulo](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), você implementará as solicitações de consultas persistentes no aplicativo WKND.
