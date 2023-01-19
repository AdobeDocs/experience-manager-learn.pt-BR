---
title: Explore a AEM API do GraphQL - Conceitos avançados de AEM headless - GraphQL
description: Envie consultas da GraphQL usando o GraphiQL IDE. Saiba mais sobre consultas avançadas usando filtros, variáveis e diretivas. Consulte referências de fragmento e conteúdo, incluindo referências de campos de texto de várias linhas.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 0%

---

# Explore a API do GraphQL AEM

A API do GraphQL no AEM permite expor os dados do fragmento de conteúdo a aplicativos de downstream. No tutorial básico [tutorial em várias etapas do GraphQL](../multi-step/explore-graphql-api.md), você usou o GraphiQL Explorer para testar e refinar as consultas do GraphQL.

Neste capítulo, você usa o explorador GraphiQL para definir consultas mais avançadas para coletar dados dos Fragmentos de conteúdo criados no [capítulo anterior](../advanced-graphql/author-content-fragments.md).

## Pré-requisitos {#prerequisites}

Este documento faz parte de um tutorial de várias partes. Certifique-se de que os capítulos anteriores foram concluídos antes de prosseguir com este capítulo.

## Objetivos {#objectives}

Neste capítulo, você aprenderá a:

* Filtrar uma lista de Fragmentos de conteúdo com referências usando variáveis de consulta
* Filtrar conteúdo dentro de uma referência de fragmento
* Consulta de conteúdo em linha e referências de fragmento de um campo de texto de várias linhas
* Consulta usando diretivas
* Consulta para o tipo de conteúdo do Objeto JSON

## Uso do explorador GraphiQL


O [Explorador GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) A ferramenta permite que os desenvolvedores criem e testem consultas contra conteúdo no ambiente de AEM atual. A ferramenta GraphiQL também permite que os usuários **persistir ou salvar** consultas a serem usadas pelos aplicativos clientes em uma configuração de produção.

Em seguida, explore o potencial AEM API do GraphQL usando o explorador GraphiQL integrado.

1. Na tela inicial AEM, navegue até **Ferramentas** > **Geral** > **Editor de consultas GraphQL**.

   ![Navegue até o GraphiQL IDE](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>No, algumas versões do AEM (6.X.X) a ferramenta GraphiQL Explorer (também conhecida como GraphiQL IDE) precisam ser instaladas manualmente, siga [instruções daqui](../how-to/install-graphiql-aem-6-5.md).

1. No canto superior direito, verifique se Endpoint está definido como **Ponto de Extremidade Compartilhado WKND**. Alteração do _Endpoint_ o valor suspenso aqui exibe a variável existente _Consultas Persistentes_ no canto superior esquerdo.

   ![Definir Endpoint GraphQL](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Isso abrangerá todas as consultas a modelos criados no **WKND Compartilhado** projeto.


## Filtrar uma lista de Fragmentos de conteúdo usando variáveis de consulta

No anterior [tutorial em várias etapas do GraphQL](../multi-step/explore-graphql-api.md), você definiu e usou consultas básicas persistentes para obter dados dos Fragmentos de conteúdo. Aqui, você amplia esse conhecimento e filtra os dados dos Fragmentos de conteúdo ao passar variáveis para as consultas persistentes.

Ao desenvolver aplicativos clientes, geralmente é necessário filtrar Fragmentos de conteúdo com base em argumentos dinâmicos. A API AEM GraphQL permite que você passe esses argumentos como variáveis em uma query para evitar a construção de string no lado do cliente no tempo de execução. Para obter mais informações sobre variáveis do GraphQL, consulte a [Documentação do GraphQL](https://graphql.org/learn/queries/#variables).

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

   O `listPersonBySkill` inclui a `contactInfo` , que é uma referência de fragmento para o modelo Informações do contato definido nos capítulos anteriores. O modelo Informações de Contato contém `phone` e `email` campos. Pelo menos um desses campos no query deve estar presente para que ele seja executado corretamente.

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

Pressione a tecla **Reproduzir** no menu superior para executar a query. Você deve ver os resultados dos fragmentos de conteúdo do capítulo anterior:

![Pessoa por Resultados de Habilidade](assets/explore-graphql-api/person-by-skill.png)

## Filtrar conteúdo dentro de uma referência de fragmento

A API AEM GraphQL permite consultar Fragmentos de conteúdo aninhados. No capítulo anterior, você adicionou três novas referências de fragmento a um Fragmento de conteúdo de empreendimento: `location`, `instructorTeam`e `administrator`. Agora, vamos filtrar todas as Aventuras de qualquer Administrador que tenha um nome específico.

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

## Consulta de referências em linha de um campo de texto de várias linhas {#query-rte-reference}

A API do GraphQL AEM permite consultar referências de conteúdo e fragmento em campos de texto de várias linhas. No capítulo anterior, você adicionou ambos os tipos de referência ao **Descrição** do Fragmento de conteúdo da equipe do Yosemite. Agora, vamos recuperar essas referências.

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

   O query também inclui a variável `_metadata` campo. Isso permite recuperar o nome do Fragmento do conteúdo da equipe e exibi-lo posteriormente no aplicativo WKND.

1. Em seguida, cole a seguinte string JSON no painel Variáveis de Consulta para obter a Aventura de Backpack do Yosemite:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
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

   O `_references` revela a imagem do logotipo e o Fragmento de conteúdo do Lodge do Vale do Yosemite que foi inserido no **Descrição** campo.


## Consulta usando diretivas

Às vezes, ao desenvolver aplicativos cliente, você precisa alterar condicionalmente a estrutura de suas consultas. Nesse caso, a API AEM GraphQL permite usar diretivas GraphQL para alterar o comportamento das consultas com base nos critérios fornecidos. Para obter mais informações sobre diretivas GraphQL, consulte o [Documentação do GraphQL](https://graphql.org/learn/queries/#directives).

No [seção anterior](#query-rte-reference), você aprendeu a consultar referências em linha em campos de texto de várias linhas. O conteúdo foi recuperado do `description` no `plaintext` formato. Em seguida, vamos expandir essa consulta e usar uma diretiva para recuperar condicionalmente `description` no `json` também.

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
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
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
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
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

   O `weatherBySeason` contém o objeto JSON adicionado ao capítulo anterior.

## Consultar todo o conteúdo de uma só vez

Até o momento, várias consultas foram executadas para ilustrar os recursos da API do GraphQL AEM.

Os mesmos dados podem ser recuperados com apenas um único query e esse query é posteriormente usado no aplicativo cliente para recuperar informações adicionais, como local, nome da equipe, membros da equipe de uma aventura:

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

## Parabéns!

Parabéns! Agora você testou consultas avançadas para coletar dados dos Fragmentos de conteúdo criados no capítulo anterior.

## Próximas etapas

No [próximo capítulo](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), você aprenderá a persistir nas consultas da GraphQL e por que é uma prática recomendada usar consultas persistentes em seus aplicativos.
