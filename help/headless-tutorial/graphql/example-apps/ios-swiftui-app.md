---
title: Aplicativo iOS - AEM exemplo autônomo
description: Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo iOS demonstra como consultar o conteúdo usando AEM APIs GraphQL usando consultas persistentes.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 3%

---

# Aplicativo iOS

Exemplos de aplicativos são uma ótima maneira de explorar os recursos headless do Adobe Experience Manager (AEM). Este aplicativo iOS demonstra como consultar o conteúdo usando AEM APIs GraphQL usando consultas persistentes.

![Aplicativo SwiftUI do iOS com AEM headless](./assets/ios-swiftui-app/ios-app.png)

Visualize o [código-fonte no GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (requer macOS)
+ [Git](https://git-scm.com/)

## Requisitos AEM

O aplicativo iOS funciona com as seguintes opções de implantação de AEM. Todas as implantações exigem o [Site WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) a ser instalado.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuração local usando [o SDK do AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR?lang=en#install-local-aem-instances)

O aplicativo iOS foi projetado para se conectar a um __Publicação do AEM__ , no entanto, ele pode originar conteúdo do Autor do AEM se a autenticação for fornecida na configuração do aplicativo iOS.

## Como usar

1. Clonar o `adobe/aem-guides-wknd-graphql` repositório:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) e abra a pasta `ios-app`
1. Modificar o arquivo `Config.xcconfig` arquivo e atualização `AEM_SCHEME` e `AEM_HOST` para corresponder ao seu serviço de publicação do AEM de destino.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   Se estiver se conectando ao autor do AEM, adicione o `AEM_AUTH_TYPE` e propriedades de autenticação compatíveis com o `Config.xcconfig`.

   __Autenticação básica__

   O `AEM_USERNAME` e `AEM_PASSWORD` autentique um usuário AEM local com acesso ao conteúdo GraphQL da WKND.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticação de token__

   O `AEM_TOKEN` é um [token de acesso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) que é autenticado para um usuário AEM com acesso ao conteúdo GraphQL WKND.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Crie o aplicativo usando o Xcode e implante o aplicativo no simulador do iOS
1. Uma lista de aventuras do site da WKND deve ser exibida no aplicativo. Selecionar uma aventura abre os detalhes da aventura. Na exibição da lista de aventuras, puxe para atualizar os dados do AEM.

## O código

Abaixo está um resumo de como o aplicativo iOS é criado, como ele se conecta ao AEM Headless para recuperar conteúdo usando consultas persistentes GraphQL e como esses dados são apresentados. O código completo pode ser encontrado em [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

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

### Executar consulta persistente de GraphQL

AEM consultas persistentes são executadas por HTTP GET e, portanto, as bibliotecas GraphQL comuns que usam HTTP POST, como Apollo, não podem ser usadas. Em vez disso, crie uma classe personalizada que execute as solicitações de HTTP GET do query persistente para AEM.

`AEM/Aem.swift` instancia o `Aem` classe usada para todas as interações com AEM Headless. O padrão é:

1. Cada query persistente tem uma função pública correspondente (por exemplo, `getAdventures(..)` ou `getAdventureBySlug(..)`) as exibições do aplicativo iOS são chamadas para obter dados de aventura.
1. A func pública chama uma func privada `makeRequest(..)` que chama uma solicitação HTTP GET assíncrona para AEM Headless e retorna os dados JSON.
1. Cada função pública decodifica os dados JSON e executa quaisquer verificações ou transformações necessárias antes de retornar os dados da Aventura para a exibição.

   + AEM dados GraphQL JSON são decodificados usando as estruturas/classes definidas em `AEM/Models.swift`, que mapeia para os objetos JSON retornou meu AEM Headless.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
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

### Modelos de dados de resposta GraphQL

A iOS prefere mapear objetos JSON para modelos de dados digitados.

O `src/AEM/Models.swift` define o [decodificável](https://developer.apple.com/documentation/swift/decodable) Estruturas e classes Swift que são mapeadas para as respostas JSON AEM retornadas por AEM respostas JSON.

### Exibições

SwiftUI é usado para as várias exibições no aplicativo. O Apple fornece um tutorial de introdução para [criação de listas e navegação com a SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   A inscrição do pedido e inclui `AdventureListView` whose `.onAppear` o manipulador de eventos é usado para buscar todos os dados de aventuras por meio de `aem.getAdventures()`. O compartilhado `aem` O objeto é inicializado aqui e exposto a outras exibições como um [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   Exibe uma lista de aventuras (com base nos dados de `aem.getAdventures()`) e exibe um item de lista para cada aventura usando o `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   Exibe cada item na lista de aventuras (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

   Exibe os detalhes de uma aventura incluindo o título, a descrição, o preço, o tipo de atividade e a imagem principal. Essa exibição consulta AEM detalhes completos da aventura usando `aem.getAdventureBySlug(slug: slug)`, em que `slug` é passado com base na linha de lista selecionada.

### Imagens remotas

As imagens referenciadas por Fragmentos de conteúdo de aventura são servidas por AEM. Este aplicativo iOS usa o caminho `_path` na resposta GraphQL e prefixos o campo `AEM_SCHEME` e `AEM_HOST` para criar um URL totalmente qualificado.

Se você se conectar aos recursos protegidos em AEM que exigem autorização, as credenciais também deverão ser adicionadas às solicitações de imagem.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) e [SDWebImage](https://github.com/SDWebImage/SDWebImage) são usadas para carregar as imagens remotas do AEM que preenchem a imagem da Adventure no `AdventureListItemView` e `AdventureDetailView` exibições.

O `aem` classe (em `AEM/Aem.swift`) facilita o uso de imagens AEM de duas maneiras:

1. `aem.imageUrl(path: String)` é usada em exibições para incluir o esquema de AEM e hospedar no caminho da imagem, criando um URL totalmente qualificado.

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. O `convenience init(..)` em `Aem` defina cabeçalhos HTTP Authorization na solicitação HTTP de imagem, com base na configuração dos aplicativos iOS.

   + If __autenticação básica__ estiver configurada, a autenticação básica será anexada a todas as solicitações de imagem.

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

   + If __autenticação de token__ estiver configurado, a autenticação de token será anexada a todas as solicitações de imagem.

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

   + If __sem autenticação__ estiver configurado, nenhuma autenticação será anexada às solicitações de imagem.



Uma abordagem semelhante pode ser usada com SwiftUI-native [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` é compatível com o iOS 15.0+.

## Recursos adicionais

+ [Introdução ao AEM Headless - Tutorial do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Tutorial de navegação e listas da interface do usuário do Swift](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
