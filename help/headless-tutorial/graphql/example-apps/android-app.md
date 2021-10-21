---
title: Aplicativo Android - Exemplo sem cabeçalho AEM
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Um aplicativo Android é fornecido e demonstra como consultar o conteúdo usando as APIs GraphQL da AEM. O Android do cliente Apollo é usado para gerar consultas GraphQL e mapear dados para objetos Swift para potencializar o aplicativo. A SwiftUI é usada para renderizar uma lista simples e uma visualização detalhada do conteúdo.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 3%

---


# Aplicativo SwiftUI do Android

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Um aplicativo Android é fornecido e demonstra como consultar o conteúdo usando as APIs GraphQL da AEM. O [Cliente autônomo do AEM para Java](https://github.com/adobe/aem-headless-client-java) O é usado para executar consultas GraphQL e mapear dados para objetos Java para potencializar o aplicativo.

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

* [Android Studio](https://developer.android.com/studio)
* [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo foi projetado para se conectar a um AEM **Publicar** com a versão mais recente do [Site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) instalado.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=pt-BR)

Recomendamos [implantação do site de referência WKND em um ambiente Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Uma configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) ou [AEM 6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) também pode ser usado.

## Como usar

1. Clonar o `aem-guides-wknd-graphql` repositório:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) e abra a pasta `android-app`
1. Modificar o arquivo `config.properties` at `app/src/main/assets/config.properties` e atualizar `contentApi.endpoint` para corresponder ao seu ambiente de AEM do target:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Baixe um [Dispositivo virtual Android](https://developer.android.com/studio/run/managing-avds) (API 28 principal)
1. Crie e implante o aplicativo usando o emulador de Android.


### Conexão com ambientes AEM

`10.0.2.2` é um [alias especial](https://developer.android.com/studio/run/emulator-networking) para localhost ao usar o emulador. So `10.0.2.2:4502` é equivalente a `localhost:4502`. Se estiver se conectando a um ambiente de publicação de AEM (recomendado), nenhuma autorização será necessária e `contentAPi.user` e `contentApi.password` pode ser deixado em branco.

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

### Buscar conteúdo

O [Cliente autônomo do AEM para Java](https://github.com/adobe/aem-headless-client-java) O é usado pelo aplicativo para executar a consulta GraphQL no AEM e carregar o conteúdo de aventura no aplicativo.

`AdventuresLoader.java` é o arquivo que busca e carrega a lista inicial de Aventuras na tela inicial do aplicativo. Utiliza [Consultas Persistentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) que [pré-embalado](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) com o site de referência WKND. O ponto de extremidade é `/wknd/adventures-all`. `AEMHeadlessClientBuilder` instancia uma nova instância com base no ponto de extremidade da api definido em `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` é o arquivo que busca e carrega o conteúdo do Adventure para cada uma das exibições de detalhes. Novamente, a variável `AEMHeadlessClient` é usada para executar a query. É executada uma consulta chartQL regular com base no caminho para o fragmento de conteúdo do Adventure. O query é mantido em um arquivo separado chamado [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` é um POJO que é inicializado com os dados JSON da solicitação GraphQL.

`RemoteImagesCache.java` é uma classe de utilitário que ajuda a preparar imagens remotas em um cache para que possam ser usadas com elementos da interface do usuário do Android. O conteúdo da Aventura faz referência a imagens no AEM Assets por meio de um URL e essa classe é usada para exibir esse conteúdo.

### Exibições

`AdventureListFragment.java` - quando chamado aciona o `AdventuresLoader` e exibe as aventuras retornadas em uma lista.

`AdventureDetailFragment.java` - inicializações `AdventureLoader` e exibe os detalhes de uma única aventura.

## Recursos adicionais

* [Introdução ao AEM Headless - Tutorial do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [Cliente autônomo do AEM para Java](https://github.com/adobe/aem-headless-client-java)

