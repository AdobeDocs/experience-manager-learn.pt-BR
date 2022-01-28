---
title: Explore a API GraphQL AEM - Conceitos avançados de AEM sem interface - GraphQL
description: Envie consultas GraphQL usando o GraphiQL IDE. Saiba mais sobre consultas avançadas usando filtros, variáveis e diretivas. Consulte referências de fragmento e conteúdo, incluindo referências de campos de texto de várias linhas.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# Explore a API GraphQL da AEM

A API GraphQL no AEM permite expor os dados do Fragmento de conteúdo aos aplicativos de downstream. No anterior [tutorial GraphQL de várias etapas](../multi-step/explore-graphql-api.md), você explorou o GraphiQL Integrated Development Environment (IDE), onde testou e refinou algumas consultas GraphQL comuns. Neste capítulo, você usará o GraphiQL IDE para explorar consultas mais avançadas para coletar dados dos Fragmentos de conteúdo criados no capítulo anterior.

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial de várias partes. Certifique-se de que os capítulos anteriores foram concluídos antes de prosseguir com este capítulo.

O GraphiQL IDE deve ser instalado antes de concluir este capítulo. Siga as instruções de instalação do [tutorial GraphQL de várias etapas](../multi-step/explore-graphql-api.md) para obter mais informações.

## Objetivos {#objectives}

Neste capítulo, você aprenderá a:

* Filtrar uma lista de Fragmentos de conteúdo com referências usando variáveis de consulta
* Filtrar conteúdo dentro de uma referência de fragmento
* Consulta de conteúdo em linha e referências de fragmento de um campo de texto de várias linhas
* Consulta usando diretivas
* Consulta para o tipo de conteúdo do Objeto JSON

## Filtrar uma lista de Fragmentos de conteúdo usando variáveis de consulta

No anterior [tutorial GraphQL de várias etapas](../multi-step/explore-graphql-api.md), você aprendeu a filtrar uma lista de Fragmentos de conteúdo. Aqui, você expandirá esse conhecimento e filtrará usando variáveis.

Ao desenvolver aplicativos clientes, na maioria dos casos, será necessário filtrar Fragmentos de conteúdo com base em argumentos dinâmicos. A API GraphQL da AEM permite que você passe esses argumentos como variáveis em uma query para evitar a construção de sequência de caracteres no lado do cliente no tempo de execução. Para obter mais informações sobre variáveis GraphQL, consulte o [Documentação GraphQL](https://graphql.org/learn/queries/#variables).

Para este exemplo, consulte todos os Instrutores que têm uma habilidade específica.

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

   O `listPersonBySkill` a query acima aceita uma variável (`skillFilter`) que é obrigatório `String`. Essa consulta realiza uma pesquisa em todos os Fragmentos de conteúdo de pessoa e os filtra com base na variável `skills` e a sequência de caracteres transmitida `skillFilter`.

   Observe que `listPersonBySkill` inclui a `contactInfo` , que é uma referência de fragmento para o modelo Informações do contato definido nos capítulos anteriores. O modelo Informações de Contato contém `phone` e `email` campos. É necessário incluir pelo menos um desses campos na query para que ele seja executado corretamente.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Em seguida, vamos definir `skillFilter` e obtenha todos os instrutores com proficiência em esquiar. Cole a seguinte string JSON no painel Variáveis de consulta no GraphiQL IDE:

   ```json
   {
   	    "skillFilter": "Skiing"
   }
   ```

1. Execute a query. O resultado deve ser semelhante ao seguinte:

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
               "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer.\nBorn in Baltimore, Maryland, Stacey is the youngest of six children. Her father was a lieutenant colonel in the US Navy and her mother was a modern dance instructor. Her family moved frequently with her father’s duty assignments, and she took her first pictures when he was stationed in Thailand. This is also where Stacey learned to rock climb."
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

## Filtrar conteúdo dentro de uma referência de fragmento

A API GraphQL AEM permite consultar Fragmentos de conteúdo aninhados. No capítulo anterior, você adicionou três novas referências de fragmento a um Fragmento de conteúdo de empreendimento: `location`, `instructorTeam`e `administrator`. Agora, vamos filtrar todas as Aventuras de qualquer Administrador que tenha um nome específico.

>[!CAUTION]
>
>Somente um modelo deve ser permitido como referência para que esta consulta execute corretamente.

1. No GraphiQL IDE, cole a seguinte consulta no painel esquerdo:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         adventureTitle
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

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta :

   ```json
   {
   	    "name": "Jacob Wester"
   }
   ```

   O `getAdventureAdministratorDetailsByAdministratorName` O query filtra todas as Aventuras para qualquer `administrator` de `fullName` &quot;Jacob Wester&quot;, retornando informações de dois Fragmentos de conteúdo aninhados: Aventura e instrutor.

1. Execute a query. O resultado deve ser semelhante ao seguinte:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "adventureTitle": "Yosemite Backpacking",
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
                         "value": "Jacob Wester has been coordinating backpacking adventures for 3 years."
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

## Consulta de referências em linha de um campo de texto de várias linhas {#query-rte-reference}

A API GraphQL da AEM permite consultar referências de conteúdo e fragmento em campos de texto de várias linhas. No capítulo anterior, você adicionou ambos os tipos de referência ao **Descrição** do Fragmento de conteúdo da equipe do Yosemite. Agora, vamos recuperar essas referências.

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

   O `getTeamByAdventurePath` O query filtra todas as Aventuras por caminho e retorna os dados para a `instructorTeam` referência de fragmento de uma Aventura específica.

   `_references` é um campo gerado pelo sistema usado para revelar referências, incluindo aquelas inseridas em campos de texto de várias linhas.

   O `getTeamByAdventurePath` O query recupera várias referências. Primeiro, ele usa o `ImageRef` objeto para recuperar o `_path` e `__typename` de imagens inseridas como referências de conteúdo no campo de texto de várias linhas. Em seguida, ele usa `LocationModel` para recuperar os dados do Fragmento de conteúdo do local inserido no mesmo campo.

   Observe que a consulta também inclui a variável `_metadata` campo. Isso permite recuperar o nome do Fragmento do conteúdo da equipe e exibi-lo posteriormente no aplicativo WKND.

1. Em seguida, cole a seguinte string JSON no painel Variáveis de Consulta para obter a Aventura de Backpack do Yosemite:

   ```json
   {
   	    "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Execute a query. O resultado deve ser semelhante ao seguinte:

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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Observe que a variável `_references` revela a imagem do logotipo e o Fragmento de conteúdo do Lodge do Vale do Yosemite que foi inserido no **Descrição** campo.


## Consulta usando diretivas

Às vezes, ao desenvolver aplicativos cliente, você precisa alterar condicionalmente a estrutura de suas consultas. Nesse caso, a API GraphQL da AEM permite usar as diretivas GraphQL para alterar o comportamento das consultas com base nos critérios fornecidos. Para obter mais informações sobre diretivas GraphQL, consulte o [Documentação GraphQL](https://graphql.org/learn/queries/#directives).

No [seção anterior](#query-rte-reference), você aprendeu a consultar referências em linha em campos de texto de várias linhas. Observe que o conteúdo foi recuperado do `description` no `plaintext` formato. Em seguida, vamos expandir essa consulta e usar uma diretiva para recuperar condicionalmente `description` no `json` também.

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

   A query acima aceita mais uma variável (`includeJson`) que é obrigatório `Boolean`, também conhecida como diretiva do query. Uma diretiva pode ser usada para incluir condicionalmente dados do `description` no campo `json` com base no booleano passado em `includeJson`.

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta :

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Execute a query. Você deve obter o mesmo resultado da seção anterior em [como consultar referências em linha em campos de texto de várias linhas](#query-rte-reference).

1. Atualize o `includeJson` diretiva `true` e execute a query novamente. O resultado deve ser semelhante ao seguinte:

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
                         "path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
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
                         "href": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Consulta para o tipo de conteúdo do Objeto JSON

Lembre-se de que, no capítulo anterior sobre a criação de Fragmentos de conteúdo, você adicionou um objeto JSON no **Tempo por temporada** campo. Agora vamos recuperar esses dados no Fragmento de conteúdo do local.

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

1. Em seguida, cole a seguinte string JSON no painel Variáveis de consulta :

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Execute a query. O resultado deve ser semelhante ao seguinte:

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
                     "value": "Yosemite National Park is in California’s Sierra Nevada mountains. It’s famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
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

   Observe que a variável `weatherBySeason` contém o objeto JSON adicionado ao capítulo anterior.

## Consultar todo o conteúdo de uma só vez

Até o momento, várias consultas foram executadas para ilustrar os recursos da API GraphQL AEM. Os mesmos dados podem ser recuperados somente com uma única consulta:

```graphql
query getAllAdventureDetails($fragmentPath: String!) {
  adventureByPath(_path: $fragmentPath){
    item {
      _path
      adventureTitle
      adventureActivity
      adventureType
      adventurePrice
      adventureTripLength
      adventureGroupSize
      adventureDifficulty
      adventurePrice
      adventurePrimaryImage{
        ...on ImageRef{
          _path
          mimeType
          width
          height
        }
      }
      adventureDescription {
        html
        json
      }
      adventureItinerary {
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
        contactInfo{
          phone
          email
        }
        locationImage{
          ...on ImageRef{
            _path
          }
        }
        weatherBySeason
        address{
            streetAddress
            city
            state
            zipCode
            country
        }
      }
      instructorTeam {
        _metadata{
            stringMetadata{
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
            profilePicture{
                ...on ImageRef {
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
        ...on ImageRef {
            _path
        mimeType
        }
        ...on LocationModel {
            _path
                __typename
        }
    }
  }
}

# in Query Variables
{
  "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
}
```

## Consultas adicionais para o aplicativo WKND

As consultas a seguir são listadas para recuperar todos os dados necessários no aplicativo WKND. Essas consultas não demonstram novos conceitos e são fornecidas apenas como referência para ajudar a criar a implementação.

1. **Obter membros da equipe para uma Aventura específica**:

   ```graphql
   query getTeamMembersByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath ) {
       item {
         instructorTeam {
           teamMembers{
             fullName
             contactInfo{
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
             biography{
               plaintext
             }
           }
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Obter caminho de localização para uma Aventura específica**

   ```graphql
   query getLocationPathByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath){
       item {
         location{
           _path  
         } 
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Obter a localização da equipe pelo seu caminho**

   ```graphql
   query getTeamLocationByLocationPath ($fragmentPath: String!){
     locationByPath (_path: $fragmentPath) {
       item {
         name
         description{
           json
         }
         contactInfo{
           phone
           email
         }
           address{
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge"
   }
   ```

## Parabéns!

Parabéns! Agora você testou consultas avançadas para coletar dados dos Fragmentos de conteúdo criados no capítulo anterior.

## Próximas etapas

No [próximo capítulo](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), você aprenderá a persistir as consultas GraphQL e por que é uma prática recomendada usar as consultas persistentes em seus aplicativos.