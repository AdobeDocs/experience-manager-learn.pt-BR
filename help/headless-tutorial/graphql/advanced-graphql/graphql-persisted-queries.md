---
title: Consultas persistentes do GraphQL - Conceitos avançados do AEM Headless - GraphQL
description: Neste capítulo de Conceitos avançados do Adobe Experience Manager (AEM) Headless, saiba como criar e atualizar consultas persistentes do GraphQL com parâmetros. Saiba como transmitir parâmetros de controle de cache em consultas persistentes.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 253
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# Consultas persistentes de GraphQL

Consultas persistentes são consultas armazenadas no servidor do Adobe Experience Manager (AEM). Os clientes podem enviar uma solicitação HTTP GET com o nome da consulta para executá-la. O benefício dessa abordagem é a capacidade de armazenamento em cache. Embora as consultas GraphQL do lado do cliente também possam ser executadas usando solicitações HTTP POST, que não podem ser armazenadas em cache, as consultas persistentes podem ser armazenadas em cache por caches HTTP ou um CDN, melhorando o desempenho. As consultas persistentes permitem simplificar suas solicitações e melhorar a segurança, pois as consultas são encapsuladas no servidor e o administrador do AEM tem controle total sobre elas. É necessário **prática recomendada e altamente recomendada** para usar consultas persistentes ao trabalhar com a API AEM GraphQL.

No capítulo anterior, você explorou algumas consultas avançadas do GraphQL para coletar dados para o aplicativo WKND. Neste capítulo, você mantém as consultas ao AEM e aprende a usar o controle de cache em consultas persistentes.

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial dividido em várias partes. Certifique-se de que o [capítulo anterior](explore-graphql-api.md) foi concluída antes de prosseguir com este capítulo.

## Objetivos {#objectives}

Neste capítulo, saiba como:

* Persistir consultas do GraphQL com parâmetros
* Usar parâmetros de controle de cache com consultas persistentes

## Revisão _Consultas persistentes do GraphQL_ definição de configuração

Vamos revisar isso _Consultas persistentes do GraphQL_ são ativados para o projeto do Site WKND na instância do AEM.

1. Navegue até **Ferramentas** > **Geral** > **Navegador de configuração**.

1. Selecionar **WKND compartilhado** e selecione **Propriedades** na barra de navegação superior para abrir as propriedades de configuração. Na página Propriedades da configuração, você deve ver que a variável **Consultas persistentes do GraphQL** a permissão está ativada.

   ![Propriedades de configuração](assets/graphql-persisted-queries/configuration-properties.png)

## Persistir consultas do GraphQL usando a ferramenta GraphiQL Explorer integrada

Nesta seção, vamos manter a consulta do GraphQL usada posteriormente no aplicativo cliente para buscar e renderizar os dados do Fragmento de conteúdo de aventura.

1. Insira a seguinte consulta no GraphiQL Explorer:

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

   ![Consulta persistente do GraphQL](assets/graphql-persisted-queries/persist-graphql-query.png)

## Execução de consulta persistente com variáveis por meio da codificação de caracteres especiais

Vamos entender como as consultas persistentes com variáveis são executadas pelo aplicativo do lado do cliente, codificando os caracteres especiais.

Para executar uma consulta persistente, o aplicativo cliente faz uma solicitação GET usando a seguinte sintaxe:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Para executar uma consulta persistente _com uma variável_, a sintaxe acima é alterada para:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

Os caracteres especiais como ponto e vírgula (;), sinal de igual (=), barras (/) e espaço devem ser convertidos para usar a codificação UTF-8 correspondente.

Executando o `getAllAdventureDetailsBySlug` Ao consultar o terminal de linha de comando, analisamos esses conceitos em ação.

1. Abra o GraphiQL Explorer e clique no link **reticências** (...) ao lado da consulta persistente `getAllAdventureDetailsBySlug`e, em seguida, clique em **Copiar URL**. Cole o URL copiado em um painel de texto; a aparência é a seguinte:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Adicionar `yosemite-backpacking` como valor de variável

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Codifique os caracteres especiais de ponto e vírgula (;) e sinal de igual (=)

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Abra um terminal de linha de comando e use [Curl](https://curl.se/) executar a consulta

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Se você estiver executando a consulta acima no ambiente de autor do AEM, será necessário enviar as credenciais. Consulte [Token de acesso de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) para a sua demonstração e [Chamar a API do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) para obter detalhes sobre a documentação.

Além disso, revise [Como executar uma consulta persistente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Uso de variáveis de consulta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables), e [Codificação do URL de consulta para uso por um aplicativo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) para saber mais sobre a execução de consulta persistente por aplicativos clientes.

## Atualizar parâmetros de controle de cache em consultas persistentes {#cache-control-all-adventures}

A API AEM GraphQL permite atualizar os parâmetros padrão de controle de cache para seus queries a fim de melhorar o desempenho. Os valores padrão de controle de cache são:

* 60 segundos é o TTL padrão (maxage=60) para o cliente (por exemplo, um navegador)

* 7200 segundos é o TTL padrão (s-maxage=7200) para o Dispatcher e o CDN; também conhecido como caches compartilhados

Use o `adventures-all` consulta para atualizar os parâmetros de controle de cache. A resposta da consulta é grande e é útil controlar sua `age` no cache. Esta consulta persistente é usada posteriormente para atualizar a [aplicativo cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Abra o GraphiQL Explorer e clique no link **reticências** (...) ao lado da consulta persistente e clique em **Cabeçalhos** para abrir **Configuração de cache** modal.

   ![Opção Persistir Cabeçalho Do GraphQL](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. No **Configuração de cache** modal, atualize o `max-age` valor do cabeçalho para `600 `segundos (10 minutos) e clique em **Salvar**

   ![Manter configuração de cache do GraphQL](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Revisão [Armazenamento em cache de consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) para obter mais informações sobre parâmetros de controle de cache default.


## Parabéns.

Parabéns! Agora você aprendeu a criar consultas persistentes do GraphQL com parâmetros, atualizar consultas persistentes e usar parâmetros de controle de cache com consultas persistentes.

## Próximas etapas

No [próximo capítulo](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), você implementará as solicitações de consultas persistentes no aplicativo WKND.
