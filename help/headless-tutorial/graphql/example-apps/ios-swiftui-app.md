---
title: Aplicativo iOS - exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo do iOS demonstra como consultar conteúdo usando as APIs do GraphQL AEM usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# aplicativo iOS

Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo do iOS demonstra como consultar conteúdo usando as APIs do GraphQL AEM usando consultas persistentes.

![Aplicativo SwiftUI para iOS com AEM Headless](./assets/ios-swiftui-app/ios-app.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Xcode](https://developer.apple.com/xcode/) (requer macOS)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo iOS funciona com as seguintes opções de implantação do AEM. Todas as implantações exigem que o [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) esteja instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)

O aplicativo do iOS foi projetado para se conectar a um ambiente __AEM do Publish AEM__. No entanto, ele poderá obter conteúdo do autor do se a autenticação for fornecida na configuração do aplicativo do iOS.

## Como usar

1. Clonar o repositório `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abrir [Xcode](https://developer.apple.com/xcode/) e abrir a pasta `ios-app`
1. Modifique o arquivo `Config.xcconfig` e atualize `AEM_SCHEME` e `AEM_HOST` para corresponder ao serviço AEM Publish de destino.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Se estiver se conectando ao Autor do AEM, adicione o `AEM_AUTH_TYPE` e as propriedades de autenticação de suporte ao `Config.xcconfig`.

   __Autenticação básica__

   O `AEM_USERNAME` e o `AEM_PASSWORD` autenticam um usuário AEM local com acesso ao conteúdo do WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticação do token__

   O `AEM_TOKEN` é um [token de acesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) que se autentica para um usuário AEM com acesso ao conteúdo do WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Crie o aplicativo usando o Xcode e implante o aplicativo no simulador do iOS
1. Uma lista de aventuras do site WKND deve ser exibida no aplicativo. Selecionar uma aventura abre os detalhes da aventura. Na exibição da lista de aventuras, puxe para atualizar os dados do AEM.

## O código

Abaixo está um resumo de como o aplicativo iOS é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado no [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Consultas persistentes

Seguindo as práticas recomendadas do AEM Headless, o aplicativo iOS usa consultas persistentes do AEM GraphQL para consultar dados de aventura. O aplicativo usa duas consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que retorna todas as aventuras no AEM com um conjunto abreviado de propriedades. Essa consulta persistente direciona a lista de aventura da visualização inicial.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura por `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente possibilita as exibições de detalhes de aventura.

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

As consultas persistentes do AEM são executadas por HTTP GET e, portanto, bibliotecas GraphQL comuns que usam POST HTTP, como Apollo, não podem ser usadas. Em vez disso, crie uma classe personalizada que execute a consulta persistente de solicitações HTTP GET para AEM.

`AEM/Aem.swift` instancia a classe `Aem` usada para todas as interações com AEM Headless. O padrão é:

1. Cada consulta persistente tem uma função pública correspondente (por exemplo, `getAdventures(..)` ou `getAdventureBySlug(..)`) as visualizações do aplicativo iOS invocam para obter dados de aventura.
1. A função pública chama uma função privada `makeRequest(..)` que invoca uma solicitação HTTP GET assíncrona para AEM Headless e retorna os dados JSON.
1. Cada função pública então decodifica os dados JSON e executa todas as verificações ou transformações necessárias antes de retornar os dados do Adventure para a exibição.

   + Os dados JSON do GraphQL do AEM são decodificados usando as structs/classes definidas em `AEM/Models.swift`, que mapeiam para os objetos JSON retornados por meu AEM Headless.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

    /// #makeRequest(..)
    /// Generic method for constructing and executing AEM GraphQL persisted queries
    private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
        // Encode optional parameters as required by AEM
        let persistedQueryParams = params.map { (param) -> String in
            encode(string: ";\(param.key)=\(param.value)")
        }.joined(separator: "")
        
        // Construct the AEM GraphQL persisted query URL, including optional query params
        let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

        var request = URLRequest(url: URL(string: url)!);

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### Modelos de dados de resposta do GraphQL

O iOS prefere mapear objetos JSON a modelos de dados digitados.

O `src/AEM/Models.swift` define as estruturas e classes Swift [decodificáveis](https://developer.apple.com/documentation/swift/decodable) que mapeiam as respostas JSON do AEM retornadas pelas respostas JSON do AEM.

### Exibições

A SwiftUI é usada para as várias exibições no aplicativo. O Apple fornece um tutorial de introdução para [criação de listas e navegação com SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  A entrada do aplicativo e inclui `AdventureListView` cujo manipulador de eventos `.onAppear` é usado para buscar todos os dados de aventuras via `aem.getAdventures()`. O objeto `aem` compartilhado é inicializado aqui e exposto a outras exibições como um [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Exibe uma lista de aventuras (com base nos dados de `aem.getAdventures()`) e exibe um item de lista para cada aventura usando o `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Exibe cada item na lista de aventuras (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Exibe os detalhes de uma aventura, incluindo o título, a descrição, o preço, o tipo de atividade e a imagem principal. Esta exibição consulta o AEM para obter detalhes completos da aventura usando `aem.getAdventureBySlug(slug: slug)`, no qual o parâmetro `slug` é passado com base na linha da lista selecionada.

### Imagens remotas

Imagens referenciadas por Fragmentos de conteúdo de aventura são servidas pelo AEM. Este aplicativo iOS usa o campo de caminho `_dynamicUrl` na resposta do GraphQL e adiciona os prefixos `AEM_SCHEME` e `AEM_HOST` para criar uma URL totalmente qualificada. Se estiver desenvolvendo no SDK do AEM, `_dynamicUrl` retornará nulo, portanto, para fallback de desenvolvimento para o campo `_path` da imagem.

Se a conexão com recursos protegidos no AEM exigir autorização, credenciais também deverão ser adicionadas às solicitações de imagem.

A [SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e a [SDWebImage](https://github.com/SDWebImage/SDWebImage) são usadas para carregar as imagens remotas do AEM que populam a imagem Adventure nas exibições `AdventureListItemView` e `AdventureDetailView`.

A classe `aem` (em `AEM/Aem.swift`) facilita o uso de imagens AEM de duas maneiras:

1. `aem.imageUrl(path: String)` é usado nas exibições para anexar o esquema e o host do AEM ao caminho da imagem, criando uma URL totalmente qualificada.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. O `convenience init(..)` em `Aem` definiu cabeçalhos de Autorização HTTP na solicitação de imagem HTTP, com base na configuração de aplicativos iOS.

   + Se a __autenticação básica__ estiver configurada, a autenticação básica será anexada a todas as solicitações de imagem.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + Se a __autenticação de token__ estiver configurada, a autenticação de token será anexada a todas as solicitações de imagem.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + Se __nenhuma autenticação__ estiver configurada, nenhuma autenticação será anexada às solicitações de imagem.

Uma abordagem semelhante pode ser usada com a [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) nativa de SwiftUI. Há suporte para `AsyncImage` no iOS 15.0+.

## Recursos adicionais

+ [Introdução ao AEM Headless - Tutorial do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR)
+ [Listas e Tutorial de Navegação da SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
