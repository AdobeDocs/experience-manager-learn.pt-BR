---
title: Aplicativo iOS - exemplo de AEM Headless
description: Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo do iOS demonstra como consultar conteúdo usando APIs AEM do GraphQL usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 3%

---

# aplicativo iOS

Aplicativos de exemplo são uma ótima maneira de explorar as capacidades headless do Adobe Experience Manager (AEM). Este aplicativo do iOS demonstra como consultar conteúdo usando APIs AEM do GraphQL usando consultas persistentes.

![Aplicativo iOS SwiftUI com AEM Headless](./assets/ios-swiftui-app/ios-app.png)

Exibir o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Xcode](https://developer.apple.com/xcode/) (requer macOS)
+ [Git](https://git-scm.com/)

## Requisitos do AEM

O aplicativo iOS funciona com as seguintes opções de implantação do AEM. Todas as implantações exigem o [Site WKND v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) a ser instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=pt-BR)
+ Configuração local usando [o AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)

O aplicativo do iOS foi projetado para se conectar a um __AEM Publish__ ambiente, no entanto, ele poderá obter conteúdo do AEM Author se a autenticação for fornecida na configuração do aplicativo do iOS.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) e abra a pasta `ios-app`
1. Modificar o arquivo `Config.xcconfig` arquivo e atualização `AEM_SCHEME` e `AEM_HOST` para corresponder ao serviço de publicação do AEM de destino.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Se estiver se conectando ao AEM Author, adicione o `AEM_AUTH_TYPE` e propriedades de autenticação de suporte para o `Config.xcconfig`.

   __Autenticação básica__

   A variável `AEM_USERNAME` e `AEM_PASSWORD` autenticar um usuário local do AEM com acesso ao conteúdo do WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticação de token__

   A variável `AEM_TOKEN` é um [token de acesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) que é autenticado em um usuário AEM com acesso ao conteúdo do WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Crie o aplicativo usando o Xcode e implante o aplicativo no simulador do iOS
1. Uma lista de aventuras do site WKND deve ser exibida no aplicativo. Selecionar uma aventura abre os detalhes da aventura. Na exibição da lista de aventuras, puxe para atualizar os dados do AEM.

## O código

Abaixo está um resumo de como o aplicativo iOS é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes do GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

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

+ `wknd/adventure-by-slug` consulta persistente, que retorna uma única aventura de `slug` (uma propriedade personalizada que identifica exclusivamente uma aventura) com um conjunto completo de propriedades. Essa consulta persistente possibilita as exibições de detalhes de aventura.

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

Consultas persistentes de AEM são executadas por HTTP GET e, portanto, bibliotecas GraphQL comuns que usam POST HTTP, como Apollo, não podem ser usadas. Em vez disso, crie uma classe personalizada que execute a consulta persistente de solicitações HTTP GET para AEM.

`AEM/Aem.swift` instancia o `Aem` classe usada para todas as interações com AEM Headless. O padrão é:

1. Cada consulta persistente tem uma função pública correspondente (por exemplo, `getAdventures(..)` ou `getAdventureBySlug(..)`) as visualizações do aplicativo iOS são invocadas para obter dados de aventura.
1. O func público chama um func privado `makeRequest(..)` que invoca uma solicitação HTTP GET assíncrona para AEM Headless e retorna os dados JSON.
1. Cada função pública então decodifica os dados JSON e executa todas as verificações ou transformações necessárias antes de retornar os dados do Adventure para a exibição.

   + Os dados JSON do AEM GraphQL são decodificados usando as structs/classes definidas em `AEM/Models.swift`, que mapeiam os objetos JSON, retornaram meu AEM Headless.

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

A variável `src/AEM/Models.swift` define o [decodificável](https://developer.apple.com/documentation/swift/decodable) Estruturas Swift e classes que mapeiam para as respostas JSON do AEM retornadas pelas respostas JSON do AEM.

### Exibições

A SwiftUI é usada para as várias exibições no aplicativo. O Apple fornece um tutorial de introdução ao [criação de listas e navegação com SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  A entrada do aplicativo e inclui `AdventureListView` cujo `.onAppear` o manipulador de eventos é usado para buscar todos os dados de aventuras por meio de `aem.getAdventures()`. O compartilhamento `aem` objeto é inicializado aqui e exposto a outras exibições como um [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Exibe uma lista de aventuras (com base nos dados de `aem.getAdventures()`) e exibe um item de lista para cada aventura usando o `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Exibe cada item na lista de aventuras (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Exibe os detalhes de uma aventura, incluindo o título, a descrição, o preço, o tipo de atividade e a imagem principal. Esta visualização consulta o AEM para obter detalhes completos sobre aventuras usando `aem.getAdventureBySlug(slug: slug)`, em que o `slug` é transmitido com base na linha da lista de seleção.

### Imagens remotas

Imagens referenciadas por Fragmentos de conteúdo de aventura são servidas pelo AEM. Este aplicativo iOS usa o caminho `_dynamicUrl` na resposta do GraphQL e adiciona os prefixos a `AEM_SCHEME` e `AEM_HOST` para criar um URL totalmente qualificado. Se estiver desenvolvendo em relação ao SDK do AEM, `_dynamicUrl` retorna nulo, portanto, para fallback de desenvolvimento para a imagem `_path` campo.

Se a conexão com recursos protegidos no AEM exigir autorização, credenciais também deverão ser adicionadas às solicitações de imagem.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e [SDWebImage](https://github.com/SDWebImage/SDWebImage) são usados para carregar as imagens remotas do AEM que preenchem a imagem do Adventure no `AdventureListItemView` e `AdventureDetailView` exibições.

A variável `aem` classe (em `AEM/Aem.swift`) facilita o uso de imagens AEM de duas maneiras:

1. `aem.imageUrl(path: String)` O é usado em exibições para anexar o esquema AEM e hospedar o caminho da imagem, criando um URL totalmente qualificado.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. A variável `convenience init(..)` in `Aem` defina cabeçalhos de Autorização HTTP na solicitação de imagem HTTP, com base na configuração de aplicativos do iOS.

   + Se __autenticação básica__ estiver configurado, a autenticação básica será anexada a todas as solicitações de imagem.

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

   + Se __autenticação de token__ estiver configurada, a autenticação de token será anexada a todas as solicitações de imagem.

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

   + Se __sem autenticação__ estiver configurado, então nenhuma autenticação será anexada às solicitações de imagem.

Uma abordagem semelhante pode ser usada com o SwiftUI-native [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` O é compatível com o iOS 15.0+.

## Recursos adicionais

+ [Introdução ao AEM headless - tutorial do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR)
+ [Tutorial de listas e navegação da SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
