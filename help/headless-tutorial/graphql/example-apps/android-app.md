---
title: Aplicativo Android - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Android demonstra como consultar o conteúdo usando as APIs GraphQL da AEM.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 0204d9aaf7b79b0745adbe749f44245716203b88
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 4%

---

# Aplicativo Android

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo Android demonstra como consultar o conteúdo usando as APIs GraphQL da AEM. O [Cliente autônomo do AEM para Java](https://github.com/adobe/aem-headless-client-java) O é usado para executar consultas GraphQL e mapear dados para objetos Java para potencializar o aplicativo.

![Aplicativo Java do Android com AEM headless](./assets/android-java-app/android-app.png)


Visualize o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## Requisitos AEM

O aplicativo Android funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem o [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) a ser instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR?lang=en#install-local-aem-instances)

O aplicativo Android foi projetado para se conectar a um __Publicação do AEM__ , no entanto, ele pode originar conteúdo do autor do AEM se a autenticação for fornecida na configuração do aplicativo Android.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) e abra a pasta `android-app`
1. Modificar o arquivo `config.properties` at `app/src/main/assets/config.properties` e atualizar `contentApi.endpoint` para corresponder ao seu ambiente de AEM do target:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __Autenticação básica__

   O `contentApi.user` e `contentApi.password` autentique um usuário AEM local com acesso ao conteúdo GraphQL da WKND.

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Baixe um [Dispositivo virtual Android](https://developer.android.com/studio/run/managing-avds) (API mínima 28).
1. Crie e implante o aplicativo usando o emulador de Android.


### Conexão com ambientes AEM

`10.0.2.2` é um [alias IP](https://developer.android.com/studio/run/emulator-networking) para localhost ao usar o emulador `10.0.2.2:4502` é equivalente a `localhost:4502`. Se estiver se conectando a um ambiente de publicação de AEM (recomendado), nenhuma autorização será necessária e `contentAPi.user` e `contentApi.password` pode ser deixado em branco.

Se estiver se conectando a um ambiente de autor de AEM [autorização](https://github.com/adobe/aem-headless-client-java#using-authorization) é obrigatório. Por padrão, o aplicativo é configurado para usar autenticação básica com um nome de usuário e senha de `admin:admin`. O [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) fornece a capacidade de usar [autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Para usar o construtor do cliente de atualização de autenticação baseada em token em `AdventureLoader.java` e `AdventuresLoader.java`:

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

Abaixo está um breve resumo dos arquivos e códigos importantes usados para potencializar o aplicativo. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Consultas persistentes

Seguindo AEM práticas recomendadas headless, o aplicativo iOS usa consultas persistentes AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventuras da exibição inicial.

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
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura por `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente potencializa as exibições de detalhes da aventura.

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
          _path
          mimeType
          width
          height
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

## Executar consulta persistente de GraphQL

AEM consultas persistentes são executadas pelo HTTP GET e, portanto, o [Cliente autônomo do AEM para Java](https://github.com/adobe/aem-headless-client-java) O é usado para executar as consultas GraphQL persistentes em relação ao AEM e carregar o conteúdo da aventura no aplicativo.

Cada consulta persistente tem uma classe de &quot;carregador&quot; correspondente, que chama de forma assíncrona o ponto final de GET HTTP AEM e retorna os dados de aventura usando o definido personalizado [modelo de dados](#data-models).

+ `loader/AdventuresLoader.java`

   Busca a lista de Aventuras na tela inicial do aplicativo usando o `wknd-shared/adventures-all` consulta persistente.

+ `loader/AdventureLoader.java`

   Obtém uma única aventura selecionando-a por meio do `slug` , usando o `wknd-shared/adventure-by-slug` consulta persistente.

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

### Modelos de dados de resposta GraphQL{#data-models}

`Adventure.java` O é um Java POJO inicializado com os dados JSON da solicitação GraphQL e modela uma aventura para uso nas visualizações do aplicativo Android.

### Exibições

O aplicativo Android usa duas visualizações para apresentar os dados de aventura na experiência móvel.

+ `AdventureListFragment.java`

   Invoca o `AdventuresLoader` e exibe as aventuras retornadas em uma lista.

+ `AdventureDetailFragment.java`

   Invoca o `AdventureLoader` usando o `slug` param transmitido por meio da seleção de aventura no `AdventureListFragment` e exibe os detalhes de uma única aventura.

### Imagens remotas

`loader/RemoteImagesCache.java` é uma classe de utilitário que ajuda a preparar imagens remotas em um cache para que possam ser usadas com elementos da interface do usuário do Android. O conteúdo de aventura faz referência a imagens no AEM Assets por meio de um URL e essa classe é usada para exibir esse conteúdo.

## Recursos adicionais

+ [Introdução ao AEM Headless - Tutorial do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Cliente autônomo do AEM para Java](https://github.com/adobe/aem-headless-client-java)
