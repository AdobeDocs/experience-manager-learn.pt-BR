---
title: Aplicativo Android - exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo do Android demonstra como consultar conteúdo usando as APIs do GraphQL do AEM.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 160
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Aplicativo Android

Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo do Android demonstra como consultar conteúdo usando as APIs do GraphQL do AEM. O [AEM Headless Client para Java](https://github.com/adobe/aem-headless-client-java) é usado para executar as consultas do GraphQL e mapear dados para objetos Java para potencializar o aplicativo.

![Aplicativo Android Java com AEM Headless](./assets/android-java-app/android-app.png)


Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo Android funciona com as seguintes opções de implantação do AEM. Todas as implantações exigem que o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) esteja instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

O aplicativo do Android foi projetado para se conectar a um ambiente __AEM do Publish AEM__. No entanto, ele poderá obter conteúdo do autor do se a autenticação for fornecida na configuração do aplicativo do Android.

## Como usar

1. Clonar o repositório `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra o [Android Studio](https://developer.android.com/studio) e abra a pasta `android-app`
1. Modifique o arquivo `config.properties` em `app/src/main/assets/config.properties` e atualize `contentApi.endpoint` para corresponder ao seu ambiente AEM de destino:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Autenticação básica__

   O `contentApi.user` e o `contentApi.password` autenticam um usuário AEM local com acesso ao conteúdo do WKND GraphQL.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. Baixe um [Dispositivo virtual do Android](https://developer.android.com/studio/run/managing-avds) (API mínima de 28).
1. Crie e implante o aplicativo usando o emulador do Android.


### Conexão com ambientes AEM

Se a conexão com um ambiente de autor AEM [autorização](https://github.com/adobe/aem-headless-client-java#using-authorization) for necessária. O [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) fornece a capacidade de usar a [autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Para usar o criador de cliente de atualização de autenticação baseada em token em `AdventureLoader.java` e `AdventuresLoader.java`:

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## O código

Abaixo está um breve resumo dos arquivos importantes e do código usado para potencializar o aplicativo. O código completo pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo iOS usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventura da visualização inicial.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura por `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente possibilita as exibições de detalhes de aventura.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
      }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
          _path
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Executar consulta persistente do GraphQL

As consultas persistentes do AEM são executadas por HTTP GET e, portanto, o [cliente AEM Headless para Java](https://github.com/adobe/aem-headless-client-java) é usado para executar as consultas persistentes do GraphQL AEM no e carregar o conteúdo de aventura no aplicativo.

Cada consulta persistente tem uma classe &quot;loader&quot; correspondente, que chama de forma assíncrona o ponto de GET HTTP AEM e retorna os dados de aventura usando o [modelo de dados](#data-models) definido pelo cliente.

+ `loader/AdventuresLoader.java`

  Obtém a lista de Aventuras na tela inicial do aplicativo usando a consulta persistente `wknd-shared/adventures-all`.

+ `loader/AdventureLoader.java`

  Busca uma única aventura selecionando-a por meio do parâmetro `slug`, usando a consulta persistente `wknd-shared/adventure-by-slug`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### Modelos de dados de resposta do GraphQL{#data-models}

`Adventure.java` é um POJO Java inicializado com os dados JSON da solicitação do GraphQL e modela uma aventura para uso nas exibições do aplicativo Android.

### Exibições

O aplicativo Android usa duas visualizações para apresentar os dados de aventura na experiência móvel.

+ `AdventureListFragment.java`

  Chama `AdventuresLoader` e exibe as aventuras retornadas em uma lista.

+ `AdventureDetailFragment.java`

  Invoca o `AdventureLoader` usando o parâmetro `slug` transmitido por meio da seleção de aventura na exibição `AdventureListFragment`, e exibe os detalhes de uma única aventura.

### Imagens remotas

`loader/RemoteImagesCache.java` é uma classe de utilitário que ajuda a preparar imagens remotas em um cache para que elas possam ser usadas com elementos da interface do usuário do Android. O conteúdo de aventura faz referência a imagens no AEM Assets por meio de um URL e essa classe é usada para exibir esse conteúdo.

## Recursos adicionais

+ [Introdução ao AEM Headless - Tutorial do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR)
+ [Cliente AEM Headless para Java](https://github.com/adobe/aem-headless-client-java)
