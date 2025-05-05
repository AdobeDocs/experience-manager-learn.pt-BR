---
title: Explore a API do AEM GraphQL - Conceitos avançados do AEM Headless - GraphQL
description: Envie consultas do GraphQL usando o GraphiQL IDE. Saiba mais sobre consultas avançadas usando filtros, variáveis e diretivas. Consulta referências de fragmento e conteúdo, incluindo referências de campos de texto de várias linhas.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
duration: 438
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# Explorar a API do AEM GraphQL

A API do GraphQL no AEM permite expor dados do Fragmento de conteúdo para aplicativos downstream. No tutorial básico [tutorial do GraphQL em várias etapas](../multi-step/explore-graphql-api.md), você usou o GraphiQL Explorer para testar e refinar as consultas do GraphQL.

Neste capítulo, você usa o GraphiQL Explorer para definir consultas mais avançadas para coletar dados dos Fragmentos de conteúdo criados no [capítulo anterior](../advanced-graphql/author-content-fragments.md).

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial dividido em várias partes. Assegure-se de que os capítulos anteriores foram completados antes de prosseguir com este capítulo.

## Objetivos {#objectives}

Neste capítulo, você aprenderá a:

* Filtrar uma lista de fragmentos de conteúdo com referências usando variáveis de consulta
* Filtrar por conteúdo em uma referência de fragmento
* Consulta para referências de conteúdo e fragmento em linha de um campo de texto de várias linhas
* Consultar usando diretivas
* Consulta para o tipo de conteúdo Objeto JSON

## Utilização do GraphiQL Explorer


A ferramenta [GraphiQL Explorer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=pt-BR) permite que os desenvolvedores criem e testem consultas em relação ao conteúdo no ambiente AEM atual. A ferramenta GraphiQL também permite que os usuários **persistam ou salvem** consultas para serem usadas por aplicativos clientes em uma configuração de produção.

Em seguida, explore o potencial da API do GraphQL do AEM usando o GraphiQL Explorer integrado.

1. Na tela inicial do AEM, navegue até **Ferramentas** > **Geral** > **Editor de consultas do GraphQL**.

   ![Navegue até o GraphiQL IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>No, algumas versões do AEM (6.X.X) da ferramenta GraphiQL Explorer (também conhecida como GraphiQL IDE) precisam ser instaladas manualmente, siga a [instrução daqui](../how-to/install-graphiql-aem-6-5.md).

1. No canto superior direito, verifique se o Ponto de extremidade está definido como **Ponto de extremidade compartilhado WKND**. Alterar o valor da lista suspensa _Ponto de extremidade_ aqui exibe as _Consultas Persistentes_ existentes no canto superior esquerdo.

   ![Definir Ponto de Extremidade do GraphQL](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Isso criará o escopo de todas as consultas para modelos criados no projeto **WKND Compartilhado**.


## Filtrar uma lista de fragmentos de conteúdo usando variáveis de consulta

No [tutorial anterior do GraphQL de várias etapas](../multi-step/explore-graphql-api.md), você definiu e usou consultas persistentes básicas para obter dados dos Fragmentos de conteúdo. Aqui, você expande esse conhecimento e filtra os dados dos Fragmentos de conteúdo transmitindo variáveis para as consultas persistentes.

Ao desenvolver aplicativos clientes, geralmente é necessário filtrar Fragmentos de conteúdo com base em argumentos dinâmicos. A API GraphQL do AEM permite passar esses argumentos como variáveis em uma query para evitar a construção de strings no lado do cliente no tempo de execução. Para obter mais informações sobre variáveis do GraphQL, consulte a [documentação do GraphQL](https://graphql.org/learn/queries/#variables).

Neste exemplo, consulte todos os instrutores que têm uma habilidade específica.

1. No GraphiQL IDE, cole a seguinte consulta no painel esquerdo:

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
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
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   A consulta `listPersonBySkill` acima aceita uma variável (`skillFilter`) que é uma `String` necessária. Esta consulta executa uma pesquisa em todos os Fragmentos de conteúdo de pessoa e os filtra com base no campo `skills` e na sequência de caracteres transmitida em `skillFilter`.

   O `listPersonBySkill` inclui a propriedade `contactInfo`, que é uma referência de fragmento para o modelo de Informações de Contato definido nos capítulos anteriores. O modelo de Informações de Contato contém `phone` e `email` campos. Pelo menos um desses campos na consulta deve estar presente para que ele seja executado corretamente.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. A seguir, vamos definir `skillFilter` e obter todos os instrutores proficientes em esqui. Cole a seguinte string JSON no painel Variáveis de consulta no GraphiQL IDE:

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. Execute a consulta. O resultado deve ser semelhante ao seguinte:

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

Pressione o botão **Reproduzir** no menu superior para executar a consulta. Você deve ver os resultados dos fragmentos de conteúdo do capítulo anterior:

![Pessoa por Resultados de Habilidade](assets/explore-graphql-api/person-by-skill.png)

## Filtrar por conteúdo em uma referência de fragmento

A API do AEM GraphQL permite consultar fragmentos de conteúdo aninhados. No capítulo anterior, você adicionou três novas referências de fragmento a um Fragmento de conteúdo de aventura: `location`, `instructorTeam` e `administrator`. Agora, vamos filtrar todas as Aventuras para qualquer Administrador que tenha um nome específico.

>[!CAUTION]
>
>Somente um modelo deve ser permitido como referência para que esta consulta seja executada corretamente.

1. No GraphiQL IDE, cole a seguinte consulta no painel esquerdo:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta:

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   A consulta `getAdventureAdministratorDetailsByAdministratorName` filtra todas as Aventuras para qualquer `administrator` de `fullName` &quot;Jacob Wester&quot;, retornando informações de dois Fragmentos de Conteúdo aninhados: Aventura e Professor.

1. Execute a consulta. O resultado deve ser semelhante ao seguinte:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## Consulta para referências em linha de um campo de texto de várias linhas {#query-rte-reference}

A API do GraphQL do AEM permite consultar conteúdo e fragmentar referências em campos de texto de várias linhas. No capítulo anterior, você adicionou ambos os tipos de referência ao campo **Descrição** do Fragmento de conteúdo da equipe do Yosemite. Agora, vamos recuperar essas referências.

1. No GraphiQL IDE, cole a seguinte consulta no painel esquerdo:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   A consulta `getTeamByAdventurePath` filtra todas as Aventuras por caminho e retorna os dados para a referência de fragmento `instructorTeam` de uma Aventura específica.

   `_references` é um campo gerado pelo sistema usado para revelar referências, incluindo aquelas inseridas em campos de texto multilinha.

   A consulta `getTeamByAdventurePath` recupera várias referências. Primeiro, ele usa o objeto `ImageRef` interno para recuperar a `_path` e a `__typename` de imagens inseridas como referências de conteúdo no campo de texto de várias linhas. Em seguida, ele usa `LocationModel` para recuperar os dados do Fragmento de conteúdo do local inserido no mesmo campo.

   A consulta também inclui o campo `_metadata`. Isso permite recuperar o nome do Fragmento de conteúdo da equipe e exibi-lo posteriormente no aplicativo WKND.

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta para obter o Yosemite Backpacking Adventure:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Execute a consulta. O resultado deve ser semelhante ao seguinte:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   O campo `_references` revela a imagem do logotipo e o Fragmento de Conteúdo do Yosemite Valley Lodge que foi inserido no campo **Descrição**.


## Consultar usando diretivas

Às vezes, ao desenvolver aplicativos clientes, é necessário alterar condicionalmente a estrutura das consultas. Nesse caso, a API GraphQL do AEM permite usar diretivas GraphQL para alterar o comportamento de suas consultas com base nos critérios fornecidos. Para obter mais informações sobre diretivas GraphQL, consulte a [documentação do GraphQL](https://graphql.org/learn/queries/#directives).

Na [seção anterior](#query-rte-reference), você aprendeu a consultar referências em linha em campos de texto de várias linhas. O conteúdo foi recuperado do campo `description` no formato `plaintext`. Em seguida, vamos expandir essa consulta e usar uma diretiva para recuperar condicionalmente `description` no formato `json` também.

1. No GraphiQL IDE, cole a seguinte consulta no painel esquerdo:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   A consulta acima aceita mais uma variável (`includeJson`) que é um `Boolean` necessário, também conhecido como a diretiva da consulta. Uma diretiva pode ser usada para incluir condicionalmente dados do campo `description` no formato `json` com base no booliano passado em `includeJson`.

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Execute a consulta. Você deve obter o mesmo resultado da seção anterior sobre [como consultar referências em linha em campos de texto de várias linhas](#query-rte-reference).

1. Atualize a diretiva `includeJson` para `true` e execute a consulta novamente. O resultado deve ser semelhante ao seguinte:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Consulta para o tipo de conteúdo Objeto JSON

Lembre-se de que, no capítulo anterior sobre criação de fragmentos de conteúdo, você adicionou um objeto JSON ao campo **Tempo por temporada**. Agora vamos recuperar esses dados no Fragmento de conteúdo do local.

1. No GraphiQL IDE, cole a seguinte consulta no painel esquerdo:

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
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
     }
   }
   ```

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Execute a consulta. O resultado deve ser semelhante ao seguinte:

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   O campo `weatherBySeason` contém o objeto JSON adicionado no capítulo anterior.

## Consulta para todo o conteúdo de uma só vez

Até o momento, várias consultas foram executadas para ilustrar os recursos da API do AEM GraphQL.

Os mesmos dados podem ser recuperados com apenas uma única consulta e essa consulta é usada posteriormente no aplicativo cliente para recuperar informações adicionais, como local, nome da equipe, membros de equipe de uma aventura:

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


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## Parabéns.

Parabéns! Agora, você testou consultas avançadas para coletar dados dos Fragmentos de conteúdo criados no capítulo anterior.

## Próximas etapas

No [próximo capítulo](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), você aprenderá a criar consultas persistentes do GraphQL e por que é uma prática recomendada usar consultas persistentes em seus aplicativos.
